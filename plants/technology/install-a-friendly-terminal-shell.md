+++
author = "Alex Bilson"
date = "2021-07-29T16:52:25"
lastmod = "2022-10-12 14:42:41"
epistemic = "evergreen"
tags = ["shell","terminal","cli"]
+++
If you're unfamiliar with a terminal command line or tired of using bash, you'll benefit from installing another shell. There are a number of options, but the fish shell is the simplest to install and use. Simple doesn't mean basic - it's elegant and wonderful.

## Install Fish

First, install fish. On MacOs the command is:

{{< highlight sh >}}
brew install fish
{{< /highlight >}}

{{< notice name="Jorge's cookbook" src="https://github.com/jorgebucaran/cookbook.fish" >}}
For an exhaustive install manual, see this cookbook.
{{< /notice >}}

You're done! Just type `fish` into your terminal and it'll switch over. If you want your terminal to start with fish by default, run this:

{{< highlight sh >}}
chsh -s /usr/local/bin/fish
{{< /highlight >}}

## Customization

Unlike any other shell, fish has a web interface to customize. Run `fish_config` and view the awesomeness!

I have a couple recommended plugins, so lets install a package manager to make this simple.

{{< highlight sh >}}
curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
{{< /highlight >}}

The prompt isn't terrible, but I prefer minimalism. For that, you'll want the hydro prompt:

{{< highlight sh >}}
fisher install jorgebucaran/hydro
{{< /highlight >}}

My most common error when typing a command is to miss a closing parenthesis or quote. Never again with:

{{< highlight sh >}}
fisher install jorgebucaran/autopair.fish
{{< /highlight >}}

