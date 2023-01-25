+++
date = "2022-01-28"
lastmod = "2023-01-25 15:02:35"
author = "Alex Bilson"
tags = [ "architecture", "efficiency", "scalability", "kubernetes",]
epistemic = "evergreen"
+++
Whether hosting a personal web service or launching entrepreneurial software, use the least number of machines to achieve your goal.

Because of the prevalence of cluster software like Kubernetes, you may conclude that the way of the future for any software is to run inside a geographically dispersed, distributed set of Kubernetes clusters. Talk of the power of microservices and the incredible feats of engineering that Google, Facebook, and Amazon have achieved with these power tools builds a lot of FOMO in our industry. But you must resist the urge.

First, you don't _really_ understand Kubernetes. You may think it's a suped-up version of container orchestration, but it's actually "a general-purpose cluster operating system kernel" ([ButtonDown](https://buttondown.email/nelhage/archive/two-reasons-kubernetes-is-so-complex/)). For what a cluster OS can achieve, it's unmatched, but you almost certainly don't need a cluster OS right now. Probably never.

Second, you underestimate the power of an individual computer. Computers at scale can do crazy stuff, but a single computer, with a little attention to maximize resource usage, can too. Expanding the number of available machines can gloss over poor design choices for a long time and burn a hole in your pocketbook. [David Crawshaw](https://crawshaw.io/blog/one-process-programming-notes) notes how a few performance adjustments can enable your startup software to manage many thousands of requests per second on a single mid-size Cloud VM.

Third, you overly respect how well large companies actually utilize their distributed compute. The mass distribution of a service does nothing for it's optimization without careful thought down to the system-level calls. Realistically, most big companies just throw money at the problem instead of digging that deep into their services. A paper by Frank McSherry, Michael Isard and Derek G. Murray explores the cost of scalability, leading their introduction with this quote:

{{< notice type=quote src="https://www.usenix.org/system/files/conference/hotos15/hotos15-paper-mcsherry.pdf" name="Paul Barham" >}}
You can have a second computer once you’ve shown you know how to use the first one.
{{< /notice >}}
