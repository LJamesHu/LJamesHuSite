---
layout: post
title:  "Five Hours to One Second: Fixing a Ten-Year-Old Scraper with Claude Code"
categories: [ technical, python, ai ]
image: assets/images/dota2_scraper_timing.png
comments: true
---
The scraper I wrote in 2015 for Torte de Lini's [Dota 2 Hero Builds Project]({% post_url 2015-05-01-dota-2-hero-builds-project %}) stopped returning anything. Not an error. It ran, said it was scraping 199 pages, and produced nothing. The tool is a Python script compiled into an exe so that Torte, who is not technical, can double-click it and get a csv of subscribers, favorites, ratings and comments for all of his guides. That's 169 guides now. It had worked, with small patches, for about ten years.

I pointed Claude Code at it and said there was an error and it wasn't returning anything.

## What actually broke

The first thing it did was fetch the listing page and look at what Steam sent back, instead of guessing at the parser. The answer was a `302` redirect to Torte's bare profile page, which says "This profile is private." His profile went private sometime after August. The workshop listing the scraper depended on is invisible to anyone who isn't logged in. In a browser it looked fine, because he *was* logged in. The individual guides were all still public. The tool just had no way to find them.

While it was in there it also found that the user-agent list had been missing commas for years, so all five agents were being sent as one enormous string, and that the "sort by subscribers" line actually sorted by unique visitors. Ten-year-old code.

## Workarounds, and why they were only okay

Steam's Dota 2 guide search works as a listing if you search for the author's name in quotes and filter the results by author. That found **167 of 169** guides. The two it missed were Ursa (1.3 million subscribers) and Spirit Breaker (3.2 million), which never show up in search no matter how you sort. So the script got a built-in list of known guide IDs and reads old output csvs as a backstop, and every guide gets verified against the author's profile link so nobody else's "Torte de Lini" guide sneaks in. Checked against the August run: 169 for 169.

Then the real wall. Steam now rate limits the community pages hard: roughly **20 page loads**, then HTTP 429 for anywhere from 10 to 50 minutes. Four runs were aborted while the retry logic got smarter. It slows itself down every time it gets limited, waits longer on each blocked retry, and eventually gives up on a page rather than hammering it. The fifth run completed. It took **5 hours 11 minutes and 41 rate limits** to scrape 169 pages.

A good chunk of that was self-inflicted. Every "let me just check this page" test request burned the same allowance the real run needed, and the blocks got longer as the day went on. Worth knowing if you ever debug a scraper against a live site.

## The keyless API: under a minute

It turns out most of what the scraper was pulling off guide pages is available from an official Steam Web API endpoint that needs no key: `ISteamRemoteStorage/GetPublishedFileDetails`. Title, hero, visitors, subscribers, favorites, 100 guides per request. Star ratings are on the listing pages, 30 per page. So the script now writes a complete csv about **one minute** in, missing only two columns (number of ratings and number of comments), then re-saves every 10 guides while it grinds through the slow pages for those. A rate-limit block or a Ctrl+C can't lose the data anymore.

## The key: under a second

A free Steam Web API key (steamcommunity.com/dev/apikey, takes two minutes) unlocks `IPublishedFileService`. `GetUserFiles` lists an author's guides even when the profile is private, and `GetDetails` with `includevotes` returns the votes and comment counts in bulk. A full run of all 169 guides now takes **0.8 seconds** and makes zero requests to the rate-limited site.

The part I actually cared about is that it didn't switch and declare victory. It ran the keyed output against the five-hour scrape, cell by cell. Star ratings and comment counts were **identical on all 169 guides**. `votes_up + votes_down` equals the "Number of Ratings" shown on the page. Stars are `ceil(score × 5)`, which it checked against 36 of ImmortalFaith's guides including the 3-star ones. Steam hides stars below roughly 25 ratings; it found that cut-off from the data (guides without stars topped out at 18 ratings, guides with stars started at 34). The exe asks for the key once, saves it next to itself, and falls back to the slow path if you press Enter. The key goes in a request header, never a URL, and never in the repo.

| | Requests to steamcommunity.com | Time | Columns |
|---|---|---|---|
| Page scrape (the original approach) | ~180 | 5 h 11 m | 9 |
| Keyless API + listing pages | ~11 | ~1 min for stats, hours for the last 2 columns | 13 |
| With an API key | 0 | 0.8 s | 31 |

## The data we never had

The old csv had nine columns because that's what the page shows. The API has about fifty fields. The ones that are actually populated are now in the output, after the original nine so nothing that reads the old format breaks.

| Column | Example (top guide, Phantom Assassin) |
|---|---|
| Votes Up / Votes Down | 583,587 / 77,109 |
| Score (Steam's 0-1 rating) | 0.8832 |
| Upvote % | 88.33 |
| Lifetime Subscribers / Lifetime Favorites | 7,703,226 / 109,977 (current: 7,545,006 / 92,246) |
| Games Played / Hours Played | 109,491,687 / 62,737,429 |
| Awards | 9 |
| Updates (times the guide was edited) | 366 |
| Role / Patch | Core / 7.41e |
| Hidden From Search | No |
| Created / Last Updated | 2013-02-23 / 2026-08-10 |

The page only ever showed a star bucket. 165 of Torte's guides are all "5 stars," but their actual scores run from 0.80 to 0.91, so now they can be ranked. Lifetime versus current subscribers gives churn. And the one I didn't expect: Steam tracks games played with the guide selected. Sessions average 26 to 38 minutes per guide, so those are Dota matches. Across all of Torte's guides that comes to **5.18 billion games** and **2.9 billion hours**. Back in 2019 I [ran a Monte Carlo simulation]({% post_url 2019-02-01-dota-2-guides-monte-carlo-simulation %}) to estimate guide usage from the outside. Turns out Valve had the number the whole time.

It also pulls the moderation fields: reports, bans, visibility, the automatic content flags and the text-check result. They're all clear today, which is exactly why they should be in a column. If one of them ever stops being zero, the run prints a "needs attention" list at the end so nobody has to open the csv to notice.

That list already has two entries. Steam flags exactly two of Torte's guides as "incompatible": Spirit Breaker and Ursa, the same two that never appeared in search. Nobody knew. I don't know what triggers the flag, but re-saving those two builds in-game is the obvious first thing to try.

## Caveats

Torte needs his own key. It's tied to a Steam account (has to have spent $5 and have the mobile authenticator), and I'm not building mine into an exe I hand to someone else. The rate-limit numbers are from one bad day on one IP address that I'd been abusing all afternoon; yours will differ. Steam's text-check result codes aren't documented anywhere, so the tool just surfaces anything non-zero for a human to look at.

And the AI sitting through a five-hour run is the least impressive part of this. The good part is that before trusting any of the fast path, it wrote offline tests with fake Steam pages and fake API responses (a banned guide, a reported guide, someone else's guide, a removed guide) and a compare script that diffed every cell of the new output against the old. That's the work I would have skipped.

## Takeaways

1. **Look at the actual response before touching the parser.** "Returns nothing" was a redirect, not a bug.
2. **Save partial output early.** A five-hour job that dies at hour four is worth nothing.
3. **Check for an official API before scraping.** It was there the whole time, and it had more data than the page did.
4. **Verify the fast path against the slow path before deleting the slow path.** The five hours weren't wasted; they were the answer key.

The code and the exe are on [GitHub](https://github.com/LJamesHu/Dota2GuideInfoScraper).

---

*Like the last two posts, this one was drafted with Claude Code from the notes of the actual session.*
