+++
author = "Alex Bilson"
date = "2021-11-18T15:38:37.364433"
lastmod = "2021-12-03 11:59:34"
epistemic = "sprout"
tags = ["javascript","software","pipeline"]
+++
It's refreshing to read Julia's article about [esbuild](https://jvns.ca/blog/2021/11/15/esbuild-vue/). I've found the JavaScript build tooling to be an obtuse, obscure mess ever since I first began to use it years ago. When that's all a person knows, like many front-end developers I've met, then it's taken for granted that this is a necessary evil. But when you've used clean, simple back-end CLI's like dotnet or scaffolded your project's pipeline with a Makefile, the wreckage that is JavaScript bundling and transpiling feels gross.

Yes, I know there are reasons it's so messy, and I'm grateful that folks made these tools to bridge the gap between the leading edge of JavaScript development and lagging browser support. [esbuild](https://esbuild.github.io/) makes me hopeful that we may, at long last, cull the outdated complexity in favor of transparent, Unixy build tooling.
