---
layout: post
title:  "Heart-icle"
categories: [ technical, python ]
image: assets/images/hearticle.png
featured: true
comments: true
---

Heart-icle is a project to solve the problem of most summaries out there. There isso much news and articles out there to read and not enough time to actually read them. Summaries on sites right now are designed to specifically NOT give you the information you want, to entice you to click in order to actually get into the meat and bones.

My proposal was to make a better, more intelligent summarizer. While there are some acceptable summarizers such as TextRank, which use a graph-based ranking model for text processing, somewhat analogous to Google's PageRank, but for text. However it isn't trained for it, just happens to leverage graphs intelligently to come up with semi-useful summary extractions, that may not be meaningful. I wanted to make a better summarizer.

The core of this idea came from observing articles on the blogging site Medium. Users on the site can highlight parts of the article, and the most highlighted segment is the "Top Highlight". The more I read articles on Medium, the more I felt that Medium highlights were actually quite representative of the article in one line. Given that I can scrape Medium articles, I can functionally have a labeled data set to train a model and use that to come up with a model specifically trained for summarization.

To do this I had to scrape over thirty thousand articles, on both Medium to get the articles and the relevant highlights, and also the Atlantic, to build up a meaningful corpus of news and article language for analysis. This was done through a combination of Selenium Webdriver and BeautifulSoup, which worked well in terms of downloading and processing a large amount of web data and stripping it down to the core article information.

To generate the summaries themselves, there were a couple of models that I was looking at, but I ended up zeroing in on Doc2Vec. It's both cutting edge, but also a new application of the tool. The canonical example for Word2Vec is the relationship between King and Queen is the same as Man and Woman.

I wanted to take this conceptual framework and apply it to articles and summaries. If I take an article and compare it to its summary, I should be able to figure out what the summarization relationship is and then utilize that to find the summary for un-summarized articles.

The results were fairly interesting, with a decent accuracy rate at roughly 10%, good for a short project trying to answer a complex question, but ultimately not good enough to feel like you could comfortably use it to generate summaries, and thus relegated to Github.

[Heart-icle Code on Github](https://github.com/LJamesHu/Heart-icle)

<embed src="https://drive.google.com/viewerng/
viewer?embedded=true&url={{site.url}}{{site.baseurl}}/assets/files/Heart-icle.pdf" style="height:600px;width:100%;">

[Link to Download PDF Presentation]({{site.baseurl}}/assets/files/Heart-icle.pdf)