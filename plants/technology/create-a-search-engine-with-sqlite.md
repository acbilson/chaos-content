+++
author = "Alex Bilson"
date = "2021-06-11T20:05:50"
lastmod = "2023-01-25 15:21:58"
epistemic = "plant"
tags = ["search","sqlite","architecture"]
+++
When I was searching for a search engine I might integrate into my website, most options felt enormously heavy. I didn't want to run a database-and-service sidecar to my site just so that I could find things. Eventually, I discovered [Lunr](https://lunrjs.com/) from [Victoria Drake](https://victoria.dev/blog/add-search-to-hugo-static-sites-with-lunr/) and had search on my website in an afternoon!

If all I needed was to search the ~500 posts on my website, Lunr would still be a perfect fit. However, I sometimes wonder if my favorite webmasters have written on a subject. Typical search engines won't surface their answers without individually asking for results from a single URL. But what if I created my own index?

To my joyful surprise, all the technology for quick full-text search is baked into the sqlite [fts5](https://www.sqlite.org/fts5.html#the_bm25_function) extension. (Note: extension sounds like a separate install, but it's included in a standard Homebrew installation).

## Scrape the Searchable Text

To use a full-text search one must first dump the text into it. However you want to do that, it's up to you. I found a Python script with the `requests` and `bs4` packages was quite sufficient for my simple needs.

## Query the Database

A proper full-text search like sqlite fts5 allows for more than a simple word search. You can find two words within six words of each other, sets of text that include two words but not one other word, etc. For my current needs, a simple word match is sufficient.

## Render the Results

Web components to the rescue! Wrap your search form and output list into a vanilla web component or use a framework like [Lit](https://lit.dev/).

## Conclusion

I thought that I'd need to re-invent some of the smarts behind a full-text search engine if I wanted something lightweight and reasonably performant to search across a couple dozen websites. But I'm happy to say that the sqlite database handles 95% of the lift and the 5% is plumbing. Win!
