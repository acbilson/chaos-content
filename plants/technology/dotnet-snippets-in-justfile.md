+++
author = "Alex Bilson"
date = "2023-03-02"
lastmod = "2023-03-02 13:13:42"
epistemic = "plant"
tags = ["docker","dotnet","justfile"]
+++
In a recent project there were a few dotnet and Docker snippets that I wrapped into Justfile commands which saved me many headaches.

{{< notice >}}
All of these commands are executed in PowerShell. You'll need to add this line to the top of your Justfile for it to use PowerShell as its default interpreter.<br /><br />
<code>set windows-powershell := true</code>
{{< /notice >}}

The first runs the project locally. Nothing special. But it also runs a pre-requisite `check` function which ensures that another service that my project depends on is running. This will _not_ be obvious to a first-time user of my project and this check lets me explicitly warn them that the project dependency is required, avoiding a lot of confusing error messages. It's also flexible enough that I can run `--verbose` or any number of additional flags without writing out the entire command.

{{< highlight sh >}}
# runs the program locally
run *ARGS: check
	dotnet run --project .\src\Console\Crawler.csproj {{ARGS}}
{{< /highlight >}}

The second launches my program inside its Docker container. It needs a user secret and different URL hosts to communicate to its dependent local services. Even though I've become adept with Docker commands, typing this whole mess out is a pain.

{{< highlight sh >}}
# starts the Docker container locally
start *ARGS: check
	$secret = (dotnet user-secrets -p .\src\Library\Lib.csproj list).Split("=")[1].Trim(); \
	docker run --rm -it --env CrawlerClientSecret=$secret ptcp/regulatory-crawler:latest ./Crawler --environment docker {{ARGS}}
{{< /highlight >}}

The third uses NSwag to generate an authenticated machine-to-machine client. What I discovered along the way, however, is that, while you can pass the project `.csproj` file via a CLI argument, NSwag isn't smart enough to put the new client code in that project's directory. Very frustrating when the CLI reports success but nothing changes because it's updated the wrong file. Am I going to remember this detail? Definitely not. But with a simple `pushd;popd` I don't need to.

{{< highlight sh >}}
# refreshes M2M client
refresh: check
	pushd src/Library; dotnet openapi refresh 'http://localhost:5700/swagger/m2m/swagger.json'; popd
{{< /highlight >}}

{{< notice >}}
Honestly, you don't need the popd since the whole command runs in a new shell that closes on completion. I like to keep commands idempotent when I can because snippets tend to get copy-pasted.
{{< /notice >}}

