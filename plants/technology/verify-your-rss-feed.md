+++
date = "2021-07-27T17:04:15"
author = "Alex Bilson"
tags = [ "rss", "javascript", "snippet",]
epistemic = "evergreen"
lastmod = "2023-03-14 16:37:10"
title = "Verify your RSS feed"
+++
If you have an RSS feed, you can use this snippet as a starting place to review the output. I'm printing the titles of each RSS item to the console, but go as deep as necessary to ensure the right information ends up in your feed.

{{< highlight js >}}
fetch('https://my-url-here.com')
.then(res => res.text())
.then(text => {
	const domParser = new DOMParser();
	const doc = domParser.parseFromString(text, 'text/html');
	var feedURL = doc.querySelector('link[type="application/rss+xml"]');
	return fetch(feedURL.href);
})
.then(resp => resp.text())
.then(text => {
	const domParser = new DOMParser();
	const doc = domParser.parseFromString(text, 'text/xml');
	const items = doc.querySelectorAll('item');
	return Array.from(items).map(item => item.querySelector('title').textContent);
}).then(titles => console.log(titles));
{{< /highlight >}}


