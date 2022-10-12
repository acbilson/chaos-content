+++
author = "Alex Bilson"
date = "2022-09-29 15:06:19"
lastmod = "2022-10-12 14:40:26"
epistemic = "plant"
tags = ["tmux","cli"]
+++
After learning how to {{< backref src="/plants/technology/save-and-restore-a-tmux-session" >}}, I began to wonder if I couldn't leverage tmux for a more project-specific use. To my joyful surprise, that's even simpler than using the plugin.

All the pieces of a tmux session can be generated from the command line, complete with options to configure them just so. To organize the command list I use my favorite command runner, [Justfile](https://github.com/casey/just). Here's an example taken from my [chaos-theme](https://github.com/acbilson/chaos-theme/blob/master/app/Justfile) repository:

{{< highlight sh >}}
# launches a tmux session with everything I need to interactively develop
develop:
	tmux new-session -s hugo -n watch -d   'just watch';
	tmux new-window  -t hugo:1 -n serve    'just serve';
	tmux new-window  -t hugo:2 -n micropub 'cd ~/source/chaos-micropub; just start';
	tmux new-window  -t hugo:3 -n edit     'nvim index.ts';
	tmux attach
{{< /highlight >}}

The first line starts a new tmux session. The window will be called "watch", and I run another Justfile command that uses `npm` to hot-serve a demo site for my web components.

The next line launches a tmux window in the same session. This Justfile command builds my web components for the demo site by {{< backref src="/plants/technology/detect-file-changes-with-entr" name="Detecting File Changes With Entr" >}}.

Some web components depend on my [micropub server](https://github.com/acbilson/chaos-micropub) to handle authentication and file access. I navigate to the repo and run another Justfile command to launch a Podman container.

Then, I open my root file with NVim. Since it's the last window created, this is the window which will be displayed when I first attach.

Finally, call `tmux attach` to start using the session. Yay!

## Why Not Docker-Compose?

When I want to run multiple interrelated services, I usually reach for `docker-compose`. One YAML file to network all the stuff together, not bad. But there are other things, like opening NVim to the source code directory, or maintaining hot reload, that are not easy or sometimes even possible in a Docker container. Tmux feels so much easier, not to mention more lightweight.
