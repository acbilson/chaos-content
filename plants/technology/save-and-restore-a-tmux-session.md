+++
author = "Alex Bilson"
date = "2022-09-14 08:49:34"
lastmod = "2022-10-12 14:40:55"
epistemic = "plant"
tags = ["tmux","cli"]
+++
As the number of running commands has increased from one to four, I've needed a better way to manage all the running terminals. I've always wanted to give `tmux` a try, and this was the perfect opportunity. But it takes a little time to run all the commands across different Git repositories so that my entire environment is ready for local development. What I needed was a way to save my session, and that's exactly what the `tmux-resurrect` plugin does.

## Installation

To install a plugin (without the package manager, which I could not get working), clone the repository:

{{< highlight sh >}}
git clone https://github.com/tmux-plugins/tmux-resurrect ~/.tmux/plugins/tmux-resurrect
{{< /highlight >}}

Then add the following line to your `~/.tmux.config` file:

{{< highlight sh >}}
run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux
{{< /highlight >}}

Finally, reload tmux with:

{{< highlight sh >}}
tmux source-file ~/.tmux.config
{{< /highlight >}}

## Usage

Once I have a session configured the way I want, I need only type `Ctrl-B Ctrl-s` to save it. Then, the next time I need to restore that session, I simply enter `Ctrl-B Ctrl-r`.
