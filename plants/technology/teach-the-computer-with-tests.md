+++
author = "Alex Bilson"
date = "2022-08-08 10:02:58"
lastmod = "2022-08-08 10:04:24"
epistemic = "sprout"
tags = ["design","programming","testing"]
+++
Thanks to Mikel Evins' post [Programming as teaching](https://mikelevins.github.io/posts/2020-02-03-programming-as-teaching/) I realized that my favored programming style is at odds with common practice. I knew something was off from the kinds of conflict I'd have with other developers, but Mikel made the difference stand out to me.

Early in my corporate programming career I became enamoured with testing. Unit tests, integration tests, component tests, you name it.

It was strange to me at that time that I hadn't cared much about testing until I joined a corporation. Although it was partly the anxiety of pushing code to production, I realize now that a big part of testing was letting me jury-rig my development style into the system.

You see, my preference is a short feedback loop where I teach the program what I want and observe the results. Any program more complex than a couple hundred lines or a couple inputs soon becomes unwieldy to run over-and-over in this fashion. But a component test lets me abstract all that complexity into stubs, then iterate on the one piece that needs work.
