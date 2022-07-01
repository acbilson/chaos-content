+++
author = "Alex Bilson"
date = "2021-09-03"
lastmod = "2022-07-01 10:31:21"
epistemic = "sprout"
tags = ["gif","cli"]
+++
I've considered the tool `imagemagick` on multiple occasions (it does practically anything you could imagine with images), but finally downloaded and tested it out. Here's how I created a useless gif animation.

First, generate two gifs.

{{< highlight sh >}}
convert -pointsize 20 -page 50x20 label:Open -append open.gif; \
convert -pointsize 20 -page 50x20 label:Close -append close.gif
{{< /highlight >}}

Next, combine them together into an animation.

{{< highlight sh >}}
convert -delay 100 -page 50x20 open.gif -page 50x20 close.gif -loop 0 animation.gif
{{< /highlight >}}

Open up `animation.gif` in your web browser and, viola, that's it!

{{< raw >}}
<img style="width: 50px" src="/data/gif/animation_example.gif"/>
{{< /raw >}}

