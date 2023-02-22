+++
author = "Alex Bilson"
date = "2023-02-22"
lastmod = "2023-02-22 15:47:30"
epistemic = "plant"
tags = ["docker","dotnet"]
+++
After discovering ways to {{< backref src="/plants/technology/shrink-your-python-dockerfile" >}}, I immediately cringed when I saw my baseline dotnet Docker image was about 200MB. How can this be slimmed? Thanks to [Brandon Atkinson](https://levelup.gitconnected.com/single-file-executables-in-net-core-3-1-and-the-quest-for-a-sub-50mb-docker-container-f44cb1274121) for the ideas.

The base Microsoft runtime image is a startling 190MB, so the first thing we'll need to do is replace this image with something a bit more slim. Alpine Linux anyone? Let's look at the minimum setup you'll need to produce a single executable for an Alpine distro.

### Publish A Single Executable

There are a few optimizations in this category to share, but first let's configure our publish command. It should look something like this:

{{< highlight dockerfile >}}
RUN dotnet publish \
	--runtime linux-musl-x64 \
	--self-contained true \
	-p:PublishSingleFile=true \
	-c Release \
	-o /app/publish crawler.csproj
{{< /highlight >}}

The most relevant flags are `--runtime`, which lets us compile our code specifically for the Alpine architecture, `--self-contained`, which publishes all the dotnet runtime dependencies along with our code, and `-p:PublishSingleFile`, which puts everything into one executable.

### Optimize Code Linking

I don't fully understand how dotnet code is compiled, but any compiler option which leaves out what isn't needed is bound to cut down on our image size. Add the following XML to the relevant `.csproj` file(s) to reduce the final output. I'm building a console app, so that'd be the console project.

{{< highlight html >}}
	<PropertyGroup>
    <PublishTrimmed>true</PublishTrimmed>
    <CrossGenDuringPublish>false</CrossGenDuringPublish>
	</PropertyGroup>
{{< /highlight >}}

### Include Runtime Dependencies

Our compiled code still expects a pile of libraries to be installed on our image, so we'll have to add those manually to Alpine. It's possible through trial-and-error that some of these might not be relevant, but that's up to you.

{{< highlight dockerfile >}}
FROM docker.io/alpine:3.16.4
COPY --from=build /app/publish .
RUN apk add --no-cache \
	openssh libunwind \
	nghttp2-libs libidn krb5-libs libuuid lttng-ust zlib \
	libstdc++ libintl \
	icu
{{< /highlight >}}

### Final Outcome

By publishing everything our code needs to run in a single executable, optimizing the final output to only the libraries that are linked, and adding a few machine dependencies, I was able to reduce the final image from 200MB to 52MB. That's four for the price of one!
