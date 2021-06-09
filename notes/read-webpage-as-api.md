+++
date = "2020-10-02T14:45:33"
tags = ["writing"]
title = "Read Webpage As API"
+++
One of the Indieweb mindsets is to treat HTML content as its own API (TODO: add link).

This snippet uses the Mozilla's Fetch API to read the content from one of my notes. Copy it into your developer console and see the content of the note appear in the console.

{{< highlight js >}}
fetch('/notes/aversions-to-the-word-evangelism').then(function(response) { return response.text(); }).then(function(text) { var mydom = new DOMParser().parseFromString(text, 'text/html'); var el = mydom.querySelector('div.e-content'); console.log(el.innerHTML.trim()); });
{{< / highlight >}}

For more fun, this snippet generates a popup whenever you hover over links set with the name 'internal'.
