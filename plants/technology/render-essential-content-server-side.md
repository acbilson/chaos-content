+++
author = "Alex Bilson"
date = "2022-01-07"
lastmod = "2022-01-07 08:57:26"
epistemic = "sprout"
tags = ["web-design","server-side"]
+++
Modern Angular and React {{< acronym SPAs "Single Page Applications" >}} compose entire websites from the client. Components construct the HTML, fetch all the data, and render the site from a bare HTML page and a JavaScript bundle. It's efficient for the programmer, but can be unnecessary or even harmful.

One of the stalwart benefits of the Internet is the ability to share what you've discovered. After all, URL stands for "Universal Resource Locator." But 100% client-side rendering collapses all states into a single resource location. Except for intentional use of query string parameters, most websites make it impossible to bookmark or share specific content within the application.

While some distribution of computing workload to the client's browser is acceptable, is it reasonable to offload the **entire** page render to their smartphone? Sometimes the answer is yes, but more often there's no business reason for the client to compose the entire site. For clients with poor Internet connections or resource-limited machines, foisting your page's hydration to their end provides a terrible experience.

By all means, let us keep building interactive websites that blend HTML, CSS and JavaScript together in brilliant ways. But perhaps we should reconsider the detriment when one of these technologies trumps the others.

{{< notice type=quote name="Jason Miller - Islands Architecture" src="https://jasonformat.com/islands-architecture/" >}}
In an "islands" model, server rendering is not a bolt-on optimization aimed at improving SEO or UX. Instead, it is a fundamental part of how pages are delivered to the browser. The HTML returned in response to navigation contains a meaningful and immediately renderable representation of the content the user requested. **Sections of that HTML may be missing their client-side interactivity, but the document should at least contain the most essential content.**
{{< /notice >}}
