
As Jan [summarizes](https://component.kitchen/blog/posts/unsurprising-code-and-magic-optimizing-for-the-first-time-vs-the-nth-time) so beautifully, optimization is a spectrum.
+++
author = "Alex Bilson"
date = "2022-12-05 08:48:29"
lastmod = "2022-12-05 08:49:04"
epistemic = "plant"
tags = ["optimization","clarity","efficiency","paradigm"]
+++

At one end of the spectrum, the greatest value of optimization is first-time clarity. A solution which can be read and understood with common industry knowledge fits into this category. Most developers know right away what the `getData` method will do:

{{< highlight js >}}
class Example {
	data: string[];

	getData() {
		fetch("http://mydata")
			.then(data => this.data);
	}
}
{{< /highlight >}}

On the other end of the spectrum, the greatest value of optimization is coding efficiency. Writing dozens of fetches with repeated urls, novel error-handling strategies and potential race conditions could be a nightmare.

{{< highlight js >}}
import Base from "./base";

class Example extends Base {
	constructor() {
		super();
	}

	doStuff() {
		if (this.data) {
			return this.data.latest;
		}
	}
}
{{< /highlight >}}

This code is more concise and still readable, but it's also not clear what's happening. What properties does data have, and when are they retrieved? I'll need to inspect the Base class at minimum to understand what's happening.

## Personal Application

Like Jan, I tend towards the flexibility side of the optimization spectrum. I'm mildly allergic to most frameworks and prefer to build everything with a high regard for first-time clarity. There's a limit of course–I'm not usually writing servers from scratch–but I generally prefer to understand and use the basic tools to perform tasks.

You could think of it in contractor terms. I keep a hammer, a screw driver, and a pair of pliers on my tool belt. When I start a project, I figure out how to use the tools I have to solve it. Since I never leave home without my three tools, I always have what I need and can solve any problem ...eventually.

Were I on the other end of the spectrum, I might have a garage full of tools. It might take a site visit to figure out which tools to bring but, after I've retrieved my impact drill, circular saw and rubber mallet, I'll have the project done faster than anyone.

(can you tell I'm not a contractor?)

This philosophy translates to other domains outside of programming and construction. I could write the same analogy in exercise using free weights and weight machines.

## Pull Requests

The theory of software optimization makes a battlefield of pull requests (PRs). When two developers are on opposite ends of the spectrum and one reviews the other's code, watch out, there are gonna be sparks.
