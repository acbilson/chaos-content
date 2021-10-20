+++
backlinks = []
date = "2021-10-26 15:36:01"
lastmod = "2021-10-26 15:52:29"
epistemic = "seedling"
tags = ["technology","pendulum","cycle"]
title = "Technology Pendulum Observation"
+++
There's a pendulum I've observed in the world of technology. It may be a law that extends beyond software developers, but it's evident from individual technologies in software development. I suspect it's a broad business principal that is more easily observed in technology because of the rapid pace of innovation.

The general shape of the pendulum is this. First, a pressing need forces software developers to implement a non-standard fix. The fix becomes so ubiquitous that soon there are libraries, sometimes even frameworks, devoted to solving that need for a mass audience. Eventually, the solution gets implemented at a core level that invalidates the need for the fix. Everyone's been using the fix for so long and the community knowledge around the fix is so foundational that there's a long trail before the fix is replaced by the core solution.

This was the road of jQuery. jQuery is a JavaScript web library that patched a gaping hole - it made simple the egregiously complex process of interacting with the DOM. The library became so popular that it ended up _everywhere_, even solving more problems than the original. The central design of jQuery, it's terse yet flexible document query functionality, was eventually incorporated into everyday browsers, but most folks continued to use jQuery long after browsers caught up. Nowadays a software developer that recommends jQuery is likely to be heckled, even if we do fondly remember how it helped up _back then_.

I propose that this pendulum has reached its peak with component frameworks. Tools like React and Angular, still wildly popular frameworks for creating frontend component architectures, can now be entirely implemented with core browser functionality. Their popularity has not yet waned because of their ubiquitous nature and the vast degree of specialization developers have devoted to learning these frameworks. They'll probably last longer _because_ they attempt to solve more than component architecture, but I don't think that will end their demise. Long hence, perhaps five to ten years even, a developer who suggests Angular as a component solution will be heckled just as the developer who recommends jQuery is today.

My takeaway from this realization is to continue innovating downward. It's more crucial to understand the problem that a current fix solves and to explore the core solutions when they're delivered than to hold tightly to any current fix. Learning how to craft components without a framework is as crucial today as it is to learn how to interact with the DOM without jQuery. At least, that's how I see it today.
