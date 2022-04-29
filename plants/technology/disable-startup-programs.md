+++
author = "Alex Bilson"
date = "2022-04-19 07:24:56"
lastmod = "2022-04-19 07:29:43"
epistemic = "plant"
tags = ["macos","cli"]
+++
From time to time on MacOS I end up with a program that decides it's so important that it deserves to open a GUI every time I log in. Annoying. These programs seem to deliberately enjoy keeping their settings out of the normal setup so that I cannot uncheck "Open at Login" in the right-click menu and disable it. But there's a CLI for that.

To see all the programs (and there's a lot), you can run the `launchctl` tool. Here's an example that'll pull up all the non-Apple startup items (about a dozen on my machine).

{{< highlight sh >}}
launchctl list | grep -v apple
{{< /highlight >}}

If you look into the /Library/LaunchAgents folder you'll find a similarly named launch file. Use the same CLI to remove the GUI from startup.

{{< highlight sh >}}
launchctl unload -w /Library/LaunchAgents/com.cisco.anyconnect.gui
{{< /highlight >}}

That's it!
