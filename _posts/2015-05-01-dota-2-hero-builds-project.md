---
layout: post
title:  "Dota 2 Hero Builds Project"
categories: [ technical, python ]
image: assets/images/dota2.png
featured: true
comments: true
---

The Dota 2 Hero Builds Project is a project my friend [Michael Cohen](https://twitter.com/tortedelini) started. I provide a lot of the technical support to his project so he can get it running smoothly. I host his site [Armchair Athleticism / Torte De Lini.com](http://tortedelini.com/) on my own VPS server on a WordPress installation. I have also provided significant data analysis assistance, being a big part of his [5 years, 350 million](http://tortedelini.com/2018/12/10/5-years-350-million-2018-dota-builds-project-year-review/), [4 years, 275 million](http://tortedelini.com/2018/02/10/4-years-275-million-2017-dota-builds-project-year-review/), [3 years, 170 million](http://tortedelini.com/2016/11/17/3-years-170-million-dota-builds-project-year-review/), [2 years, 100 million](http://tortedelini.com/2015/10/28/2-years-100-million-dota-builds-project-overview/), and [1 Year, 40 million](http://tortedelini.com/2014/11/17/1-year-40-million-dota-builds-project-overview/) subscriptions benchmark articles and analysis, assisting in the data visualization and writing.

To assist in getting statistics for all of the over 100 guides, I created a data collection scraper and analyzer for near instant calculation and parsing of the data. Normally it would take up to an hour to manually calculate and pull data every 2 weeks, which was automated into a single script that collects all the data and outputs a csv for intake.

[Dota 2 Guide Info Scraper on GitHub](https://github.com/LJamesHu/Dota2GuideInfoScraper)

The code was written in Python and relied heavily on the BeautifulSoup library to pull information from each of this guides. This was then all compiled into an exe file so Torte de Lini could quickly use it without any Python setup or additional files.

<embed src="https://drive.google.com/viewerng/
viewer?embedded=true&url={{site.baseurl}}/assets/files/guideData-2015-11-14-02-18-12.pdf" style="height:600px;width:100%;">

[Link to Download Sample CSV Data]({{site.baseurl}}/assets/files/guideData-2015-11-14-02-18-12.csv)

In addition to that, since Torte De Lini has many guides, so it is rather annoying to get people to subscribe to, favorite, or rate his guides. I learned how to make bookmarklets and designed a bookmarklet that allowed people to like, favorite and subscribe to all of his guides in one button press. It was a rather interesting project since I was relatively unfamiliar with bookmarklets and had to figure out how to set it up to work correctly.

[Dota2SubLikeFav Bookmarklets on GitHub](https://github.com/LJamesHu/Dota2SubLikeFav)