---
layout: post
title:  "Dota Guide Monte Carlo Simulation"
categories: [ technical, python ]
image: assets/images/tax_v_corruption.png
comments: true
---

Torte de Lini, an old friend that I've also been working with to set up technical infrastructure for his Dota 2 project, approached me with a new, interesting question. Given that his guides have so many subscribers, one valid question is, how often, and how many people use his guides in a game? While we can't get the exact numbers, we can come up with a rough estimation given a bunch of numbers that he's been collecting.

Given that we know that how many games are played daily (dota.rgp.io), how many games are played for each hero (Dotabuff.com), and how many games for a specific hero use a Torte de Lini guide by day (available in-game), we can then get an estimate of how many games have at least one person using a Torte de Lini guide.

This was calculated and estimated by using a Monte Carlo simulation method. While it could theoretically be calculated to exact precision given that we have the percents, there are a possible 81,572,506,886,508 possible team combinations, which is an unrealistic amount of calculations required for very little upside. The Monte Carlo simulation created teams of 10 different heroes, based on play rate and calculated the probability of that person using a guide given the numbers provided. This was then run 100,000 times and stored.

Across multiple tests, the percents were fairly stable. So the Monte Carlo simulation shows that roughly 17% of games have no one in the game using a guide, but in the other 83%, at least 1 person uses a guide, which is a pretty remarkable number.

To semi-validate this data, given the numbers we have, it seems like roughly 15% of all games across all heroes use a Torte de Lini guide, which means that for any individual hero, 85% of games do not. If we take that basic assumption given all heroes are picked equally, the probability of a game having no one using a guide is .85 ^ 10 or 19.68%. Not too far off from what our Monte Carlo simulation shows, so the results look fairly reasonable. So even though only 15% of games played on any individual hero uses a Torte de Lini guide, the combined probability of among 10 people, at least 1 person using a guide is around 83%.

Some assumptions are made that are not representative of the real world. Heroes were selected at random, whereas more realistically, you would assume some level of team composition. There is also the assumption that players will use guides at random, whereas the truth is probably less skilled players use guides more often than skilled players. So realistically, more than 17% of games will have no one using a guide, but there is no better way to calculate this skill based guide usage.

Given the prevalence of the guides' usage, the results weren't too surprising. His guides are some of the most popular tools in the Dota 2 community, having reached pretty far and wide for a free project inside Dota 2.