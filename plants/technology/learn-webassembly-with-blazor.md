+++
author = "Alex Bilson"
date = "2021-11-17T19:01:16.379390"
lastmod = "2021-11-30 08:04:59"
epistemic = "seedling"
tags = [ "wasm", "blazor", "csharp",]
title = "Learn WebAssembly With Blazor"
+++
My manager asked me today what technology I'd like to explore. Could be anything, just have to bring back something for my team to learn. The first topic that came to mind was browser-native web components, but he suggested I look into WebAssembly. Since wasm has greater implications for my day-to-day work, I agreed to take it on.

These are my scratch notes and I learn how it's done.

# Examples

- [Blazor WASM TODO Tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/build-a-blazor-app?view=aspnetcore-6.0&pivots=webassembly)
- [Blazor Web API Tutorial](https://docs.microsoft.com/en-us/aspnet/core/blazor/call-web-api?view=aspnetcore-6.0&pivots=webassembly)

While the basic tutorial looks fairly easy, it's crucial to have a service call to demonstrate the full power of what's possible. Practically every production-ready web app needs to fetch data from a backend database, so my learning must include this step to be worthwhile.

Only related to the Web API portion, Scott Hanselman wrote a helpful overview of the latest C# code for a [Minimal Web API](https://www.hanselman.com/blog/exploring-a-minimal-web-api-with-aspnet-core-6).
