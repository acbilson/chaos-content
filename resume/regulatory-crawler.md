+++
author = "Alex Bilson"
date = "2023-03-02"
lastmod = "2023-03-02 13:28:29"
+++

## Purpose

Our advisors regularly scan three websites for new regulatory information that's relevant to the banks and credit unions that we serve. There are thousands of regulatory articles, so it's a lot to dig through. Relevant articles are manually added to our system for clients to review. But it's a tedious process that prevents our advisors from more valuable work.

The regulatory crawler project's aim is to find new regulatory information and load it into our system so that our advisors merely need to approve articles for them to be highlighted to our clients.

## Design

There's a lot of surface area to cover for a crawler project like this one. Here's a limited breakdown:

1. Our system's Angular frontend needs to be modified to allow advisors to approve articles.
2. Our C# backend service needs a new authenticated machine-to-machine endpoint for the crawler to retrieve the existing regulatory articles and to add new ones.
3. The crawler itself must be built. It must retrieve data from the three regulatory websites, normalize their responses into our system's model, check which aren't already loaded, then add the new articles.

The technology to operate this is diverse. There's the Angular JavaScript framework. There's C# as it's used for web services and C# as it's used for container-level console applications. There's OpenAPI configurations to enable the crawler to generate an authenticated machine-to-machine client. Finally, there's the mashup of technologies that make up containerization: Docker, Kubernetes manifests, etc.

## Highlights

Alright, let's get a little nerdy. I'm going to demonstrate solutions to a few different problems I encountered along the way.

### The dotnet CLI

I'm a reluctant user of Visual Studio. The way it hides what it's doing behind a UIT makes me uncomfortable. Having written code in Rust, Go, and Python without any IDE dependencies, it feels confining to be locked into a magical (and resource-intensive) tool to build a small crawler. However, my experience with the dotnet CLI is limited and I know there'll be some command configuration I'll have to learn and repeat. Enter one of my favorite tools, [Justfile](https://github.com/casey/just).

{{< backref src="/plants/technology/dotnet-snippets-in-justfile" name="Elsewhere" >}} I've written down a few snippets that saved me hours of time running dotnet commands. Putting them into a Justfile also equips whoever looks at this project in the future (myself included) to easily run an assortment of common commands along with all the most helpful and necessary flags.

## Dockerfile

When I had the most basic Console app imaginable I wrote up the Dockerfile and generated my image. To my dismay the thing was 200MB! With a little investigation I found some {{< backref src="/plants/technology/slim-your-dotnet-dockerfile" name="configuration steps" >}} that brought the final output down to 52MB.

After adding the NSwag dependencies I was no longer able to trim the published code with IL Linker. It's not completely clear to me why that's so, but it's a significant liability that any package added to my project might force me to accept 100MB of unused libraries. Now my bloated Docker image sits at 163MB. Not cool 🙁.

## Unit Testing

I'm a huge fan of unit tests, so no project lasts long before I'm refactoring to make it testable. I was pleasantly surprised to discover that the host configuration tools which Microsoft produces and which often baffle me make writing testable code easy. So this is how to {{< backref src="/plants/technology/test-a-dotnet-web-service" >}}.
