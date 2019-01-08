---
layout: post
title:  "Heart-icle"
categories: [ technical, python ]
image: assets/images/hearticle.png
featured: true
comments: true
---

* Used semi-supervised learning with Medium’s user generated highlights to create better summaries.
* Scraped over 30,000 articles from Medium and The Atlantic using BeautifulSoup and Selenium Webdriver.
* Integrated Gensim’s Doc2Vec and Medium highlight data to capture the relationship between an article
and highlight, resulting in a model that uses vector math to calculate highlights from inputted articles.

[Heart-icle Code on Github](https://github.com/LJamesHu/Heart-icle)

<embed src="https://drive.google.com/viewerng/
viewer?embedded=true&url={{site.baseurl}}/assets/files/Heart-icle.pdf" style="height:600px;width:100%;">

[Link to Download PDF Report]({{site.baseurl}}/assets/files/Heart-icle.pdf)