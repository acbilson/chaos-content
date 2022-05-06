+++
author = "Alex Bilson"
date = "2022-05-06 08:12:29"
lastmod = "2022-05-06 08:14:53"
epistemic = "plant"
tags = ["shell","configuration"]
+++
I enjoy using the fish prompt, and fish_config is brilliant. But if I want to swap my remote terminal to another prompt but don't want to open a hole in my firewall to run the config web page, it's not obvious how I can do that. So, for posterity, here's what you do.

All the starter prompts are located at the following path. Execute them as a script and, boom, now your prompt will be set to that default. For example, to switch to the terlar prompt, run:

{{< highlight sh >}}
. /usr/share/fish/tools/web_config/sample_prompts/terlar.fish
{{< /highlight >}}
