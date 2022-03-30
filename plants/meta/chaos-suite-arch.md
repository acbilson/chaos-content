+++
author = "Alex Bilson"
date = "2022-03-04"
lastmod = "2022-03-30 15:34:39"
epistemic = "seedling"
tags = ["web-design","architecture"]
+++
Re-inspired by Linus Lee, I'm thinking about what's left to fill out the chaos architecture.

## Authentication

First, I require infrastructure to authenticate parts of my chaos services, preferably without implementing authentication for each individually. I am inspired by [Vouch Proxy](https://github.com/vouch/vouch-proxy), but I have not presently been able to install it on a 32-bit ARM architecture. However, implementing the Auth Request Module interface doesn't seem impossibly hard; [here](https://redbyte.eu/en/blog/using-the-nginx-auth-request-module/) is an example in Golang. I might use Python instead since I already have a working OAuth model (unless I can find an easy library in another language).

## Standardization

Second, I need to standardize the core technologies across all my services. Some of them are wrappers around other's open-source code - these don't need to be changed - but my custom services need standardization. That doesn't mean that I need to do a re-write today - isn't that part of what makes containers wonderful? - but I shouldn't choose technology that I wouldn't use for any service. The current options:

### Backend/frontend languages and frameworks

#### Python / Flask

This is what I've written some services in already. I'm familiar with Flask and comfortable with many of the framework plugins. There's decent tooling support and it's not bloated with extra crap. However, it builds exceptionally large container images and, while I haven't had much trouble with Jinja templates, they still feel a little magical. It has some of the most useful documentation of any I've seen, however, so I have a strong feeling that I can solve problems that arise.

#### Golang / Some Framework

I have written some Golang code and have really enjoyed it. It builds beautifully compact container images and, while I'm more familiar with C# and Python than Golang, cross-over knowledge is multiplying benefits at this point. The biggest obstacle here is having no familiarity with a server-side web rendering engine. There's Flask for Python and .NET Razor for C#, but what would I need to learn for a Golang web app? I've looked at Tonic a bit. The other positive to using Golang is that more of the open-source projects that I use have Golang source code than any other.

#### C# / Blazor Server

I've tried a little of Blazor Server, and the results have been really nice. I'm already intimately familiar with C#, both the code and also .NET Core usage. The bleeding-edge nature of Blazor gives me a little pause (does this have longevity?), and the C# container images are somewhere between Python and Golang, but I could easily build single container images with a SQLite backend. I'm also familiar with NuGet which means, theoretically, that I might have an easier time with code dependency sharing than other languages, maybe. Another possible plus is that a corpus of C# open source code can't hurt my chances of finding more work. I've enjoyed using both Python and Golang, and will continue to use them (plus Rust and whatever else), but I don't have any aspiration to work at a Golang or Python business. That wasn't true before .NET Core (I _hated_ the lack of interoperability), but now it's becoming truly OS-independent and only growing more so with technology like WebAssembly.

For the few open-source technologies, knowing what they're written in would help if I ever wanted to fork my own version...

- beancount/fava = Python, a core of C/C++ and Svelte
- adnanh/webhook = Golang
- miniflux/v2 = Golang

### Alternative to Standardization

The purpose of standardization across services is mostly to reduce cognitive load, with a secondary need for shared libraries. If the underlying code is not standardized, an alternative solution would be to standardize the inputs and outputs with a middleware web service. However, this is tricky because of the sheer number of options. These are in no particular order or compatibility:

- Each web service can implement structured data with microformats2 in the HTML. This has the advantage that each service is truly stand-alone because it doesn't merely deliver a JSON payload, it delivers entire web pages. The page itself is the endpoint, able to be read by anything that can parse microformat2. It also institutes more duplication, however, since a change might be implemented in the service template, then in the middleware.
- Each web service might be reduced to a JSON payload, and a middleware web service might render the pages. The middleware code would only need to change if I wanted to update the UI or if I needed to make a service change.

But perhaps a middleware is getting too far ahead of myself. Being able to read from each of these sources is mostly valuable for search and aggregation but there's no reason I could not utilize each web service independently - that's usually what I do. Solving the authentication piece makes this even simpler because I can adjust access simply by adding redirects to the respective Nginx config files.

### Data Warehousing

Until this point I've managed to operate all of my services without a database, only the Markdown files in chaos-content. There's a Postgres database for the feedreader, but it's from the open-source implementation and I've had no trouble with it. However, storing structured data is becoming an increasingly felt need and, with the growing need comes a host of other concerns about data integrity, migration, and backup. Because my Markdown files are stored in Github, that's been the extend of my backup and migration solution. Pull down the latest, off we go. I've had minimal compatibility problems with the Hugo chaos-theme and the content because I could deploy both together quickly.

In this line I'm much in preference of SQLite. It's the easiest to backup, copy, and duplicate. I don't fully understand how I'd achieve a migration strategy, but I may not need to solve that for months to come (if ever). The databases can also remain inside the container that uses them. I don't need multiple concurrent copies, so that's perfect.

### Just Add More Open Source Services

Before I truly decide if I _want_ to go this route, it may be worthwhile to search around for existing services I could run for a few more needs. A CMS is a good example of something I'd like to try out before buying. Since most of the fun is in the journey, however, I may prefer to build it anyways.

### End Game - Search

For some time now I've benefited from having full web access to my micropub service. I just write down a lot more when there's a low-friction means to do so and it all lands in the same place. But there are other data sources that, if I were able to access them from a single search engine, could add a lot to my workflow. But at this point those data sources must be private (email, contact lists, etc.). That's the end-game, perhaps, to have all my data within reach. IDK.

{{< image "/data/svg/chaos-suite.svg" "chaos-suite" >}}
