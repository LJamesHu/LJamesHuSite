---
layout: post
title:  "Making A New Personal Site - Jekyll Edition"
categories: [ technical ]
image: assets/images/new_jekyll_site_2018.png
featured: true
comments: true
---
Profile sites are basically a person's way of interacting and showing the outside world the kind of work you've done. I've had a profile site for a while but never really "loved" it. I've had a website since 2016, to serve as a place to link to when showing people work and also have an email that isn't **@gmail.com**.

I had a site made by my good friend [Zak Tan](https://websitedaytona.com/) and it worked for me at the time, but didn't quite meet my vision, and I wasn't sure how to move on from there. Most sites are very image heavy, but as a data scientist, my work really wasn't image heavy overall. I wanted images to play back seat to content.

![Old LJamesHu Wordpress Site]({{site.baseurl}}/assets/images/old_wp_site_2016.png)

Technologically, it also felt a little problematic for me. It was based off Wordpress, which is a fine technology stack, but didn't work for me. I didn't really want to keep hosting a Wordpress server that was both cumbersome to manage, but also had so many extra features I wasn't taking advantage of, and was too difficult for me to modify. If I wanted to change things, the code was a little too bespoke for me to change without learning the full stack. I also had little to no desire to learn PHP, arguably a dying language, and definitely not something really used by data scientists.

Looking around at the options, a lot of things pointed to using Jekyll. While it's a bit played out, Jekyll is the language of tech people, who don't need a WYSIWG editor, who want to get things done quickly, who want to host on GitHub Pages for free.

After having done some work with getting the [Luminar Extra Life Site]({% post_url 2017-02-01-extra-life-2016 %}) online, I felt like GitHub would be the right place to have host my new site.

Additionally, I drew a lot of inspiration from [Lilian Chen](http://www.lilchen.com/blog/site-redesign-process/). The website style was very close to what I wanted although far more artistic than I had the ability to continuously do. I wanted to try and tweak her design into something that I would love to use. But it did give me a sense of what I wanted and as a not designer, her breakdown of her approach helped me also refine what I was looking for, and what I didn't really care about.

After all of that, Jekyll seemed like the obvious choice. I actually initially tried copying her code directly, but it clearly wasn't designed to be completely understood by a beginner, or to even be copied. Which is fair, unless you're planning on sharing something, there really isn't a huge point in generalizing things for other people.

That, combined with Jekyll being relatively poorly documented in my opinion, and it's just difficult to get things set up. I had to start with something... simpler so I took a look at free Jekyll templates, and immediately [Mediumish](https://github.com/wowthemesnet/mediumish-theme-jekyll/), one of the most used Jekyll templates, spoke to me. Visually it wasn't quite what I wanted, much more image forward, but structurally it was designed to be copied and modified, and I felt like I could take it the last mile.

![Mediumish Jekyll Template]({{site.baseurl}}/assets/images/mediumish-jekyll-template.png)

But in order to reach the certain vision I wanted to implement, it proved to be fairly difficult. Jekyll tech is based off of a combination of Ruby, Liquid templating, and Markdown for the last mile. For someone who has never done this before... it proved... confusing. Most of my experience programming would be in the form of analysis, not design so all of this was new to me.

Ultimately, took me a lot of work, lots of googling, but it got to the site that you see now. Now this may change in the future, but this seems pretty good to me. It's not a image forward, but rather more text forward and really serves some of the things that I immediately wanted.