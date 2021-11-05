+++
author = "Alex Bilson"
backlinks = [ "/notes/craft-your-own-site",]
comments = false
date = "2020-10-02T14:45:33"
epistemic = "plant"
lastmod = "2021-11-05T14:13:36.080431"
tags = [ "writing", "javascript",]
title = "Read Webpage As API"
+++
One of the Indieweb mindsets is to treat HTML content as its own API.

## HTML-only content is eminently sharable

Content that sits in a database is sharable, but not eminently so. Databases are queried via an API that's usually not publicly available and documented. While the user may comfortably use the API to render content to their site, my interface is the site itself rather than the underlying API. Lazy-load data via JavaScript and, well, it's not eminently sharable.

In contrast, structured HTML is available to anyone with web browser. A developer can look at the structure of my site and query the same type of information from any page. This is marvelously sharable.

While it's true that a person _could_ mark up database-driven HTML to be queried by external sources, a sort of public API to replicate their private API, it's inevitable that one of these APIs will atrophy. Always the unused one. This is why, not only do I publish API-able HTML, but I depend on it.

## Example JavaScript Snippet

This oneline snippet uses the Mozilla's Fetch API to read the content from one of my notes and display it in the developer console. Copy it into your developer console and see the content of the note appear in the console.

{{< highlight js >}}
fetch('/notes/aversions-to-the-word-evangelism').then((response) => response.text()).then((text) => { var mydom = new DOMParser().parseFromString(text, 'text/html'); var el = mydom.querySelector('div.e-content'); console.log(el.innerHTML.trim()); });
{{< / highlight >}}

I've used the same philosophy to {{< backref src="/notes/display-backlink-preview-on-hover" >}}
