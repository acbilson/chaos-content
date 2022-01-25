+++
author = "Alex Bilson"
date = "2022-01-28"
lastmod = "2022-01-28 09:01:45"
epistemic = "sprout"
tags = ["architecture","efficiency","scalability","kubernetes"]
+++
Whether hosting a personal web service or launching entrepreneurial software, use the least number of machines to achieve your goal.

Because of the prevalence of cluster software like Kubernetes, you may conclude that the way of the future for any software is to run inside a geographically dispersed, distributed set of Kubernetes clusters. Talk of the power of microservices and the incredible feats of engineering that Google, Facebook, and Amazon have achieved with these power tools builds a lot of FOMO in our industry. But you must resist the urge.

First, you don't _really_ understand Kubernetes. You may think it's a suped-up version of container orchestration, but it's actually "a general-purpose cluster operating system kernel" ([ButtonDown](https://buttondown.email/nelhage/archive/two-reasons-kubernetes-is-so-complex/). For what a cluster OS, it's unmatched, but you almost certainly don't need a cluster OS right now. Probably never.

Second, you underestimate the power of an individual computer. Computers at scale can do crazy stuff, but a single computer, with a little attention to maximize resource usage, can too. Expanding the number of available machines can gloss over poor design choices for a long time and burn a hole in your pocketbook. [David Crawshaw](https://crawshaw.io/blog/one-process-programming-notes) notes how a few performance adjustments can enable your startup software to manage many thousands of requests per second on a single mid-size Cloud VM.
