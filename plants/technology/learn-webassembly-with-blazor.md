+++
author = "Alex Bilson"
date = "2021-11-17T19:01:16.379390"
lastmod = "2022-03-30 15:25:34"
epistemic = "seedling"
tags = [ "wasm", "blazor", "csharp",]
title = "Learn WebAssembly With Blazor"
+++
My manager asked me today what technology I'd like to explore. Could be anything, just have to bring back something for my team to learn. The first topic that came to mind was browser-native web components, but he suggested I look into WebAssembly. Since wasm has greater implications for my day-to-day work, I agreed to take it on.

These are my scratch notes and I learn how it's done by building a POC called [fire-tables](http://github.com/acbilson/fire-tables).

## Create .NET Project with Docker

To generate the base files without having to install dotnet on my Mac, I ran the following two commands. The first runs the create command on a dotnet 6.0 sdk image, downloading it if you don't already have it. The second copies the results to a /src folder in your local directory, creating it if it doesn't exist.

{{< highlight sh >}}
docker run --name blazor mcr.microsoft.com/dotnet/sdk:6.0 dotnet new blazorwasm -o /src
docker cp blazor:/src .
{{< /highlight >}}

## Configure Blazor for Docker

At least for a start you'll need a Docker image to run Blazor in local development. Here's a sample Dockerfile, short and sweet.

{{< highlight sh >}}
FROM mcr.microsoft.com/dotnet/sdk:6.0 as base
WORKDIR /src

FROM base as dev
ENTRYPOINT ["dotnet", "run"]
{{< /highlight >}}

If you've already generated your project, you'll need to set the applicationUrl in your Properties/launchSettings.json to `http://0.0.0.0:80`. I also turned off launchBrowser since the image is only serving the route, not the browser.

That's everything you'll need to run this puppy locally. Not bad, huh? Here's a sample build/run command to get off the ground.

{{< notice type=warn >}}
Don't forget to replace 'my-name-here' with your name!
{{< /notice >}}

{{< highlight sh >}}
docker build -f Dockerfile \
    --target=dev \
    --build-arg EXPOSED_PORT=80 \
    -t my-name-here/blazor-client-dev:6.0 .
{{< /highlight >}}

This build handles exposing the container's PORT 80 outside the Dockerfile. You could include that line in the Dockerfile and remove it here, but I find it nice to be explicit.

{{< highlight sh >}}
docker run -it --rm \
  --expose 80 -p 8080:80 \
  -v src:/src \
  --name blazor-client \
  my-name-here/blazor-client-dev:6.0
{{< /highlight >}}

We're serving the files directly from your fileshare since this is a development example. Pull up `http://localhost:8080` to see your handiwork.

## Examples

- [Blazor WASM TODO Tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/build-a-blazor-app?view=aspnetcore-6.0&pivots=webassembly)
- [Blazor Web API Tutorial](https://docs.microsoft.com/en-us/aspnet/core/blazor/call-web-api?view=aspnetcore-6.0&pivots=webassembly)

While the basic tutorial looks fairly easy, it's crucial to have a service call to demonstrate the full power of what's possible. Practically every production-ready web app needs to fetch data from a backend database, so my learning must include this step to be worthwhile.

Only related to the Web API portion, Scott Hanselman wrote a helpful overview of the latest C# code for a [Minimal Web API](https://www.hanselman.com/blog/exploring-a-minimal-web-api-with-aspnet-core-6).

## Latest

After beginning down the path of Blazor WebAssembly with a .NET Core Server backend, I realized that there was little benefit to building these separately (at least for this project). If I wanted to build a separate client and server I might just use Angular and .NET Core instead. So I've decided to pursue Blazor Server, which operates similarly to an updated version of ASP.NET Web Forms.

## Install NuGet Packages

I'm not using Visual Studio for this project, which is forcing me to learn the CLIs. Fortunately, the Microsoft CLIs have come a _long_ way since I first tried to use them pre-dotnet.

One of the first things I needed to figure out was how to install the Entity Framework 6 NuGet packages (I want to use this ORM with a SQLite backend for data persistence). Turns out, that's a cinch.

{{< highlight sh >}}
dotnet package add System.Data.SQLite
dotnet package add System.Data.SQLite.Core
dotnet package add System.Data.SQLite.EF6
{{< /highlight >}}

## Further Resources

- https://github.com/CuriousDrive/BlazingChat
- https://github.com/JeremyLikness/PlanetaryDocs
- https://github.com/chrissainty/AuthenticationWithClientSideBlazor
