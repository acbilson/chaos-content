+++
author = "Alex Bilson"
date = "2022-06-03 10:40:03"
lastmod = "2022-10-12 14:45:31"
epistemic = "plant"
tags = ["ab","benchmark","cli"]
+++
Time to test your web service with more than a single request? Let's use `ab`!

Here's a silly example.

{{< highlight sh >}}
ab -n 1000 -c 100 localhost:3000/healthcheck
{{< /highlight >}}

This runs 1,000 requests at a rate of approx. 100 at a time. It runs on a single thread so it's not technically 100 at the same time, but rather in bursts of 100.

You'll get a nifty breakdown of requests success/failure and the distribution of response times in milliseconds.
