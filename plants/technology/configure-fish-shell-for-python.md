+++
author = "Alex Bilson"
date = "2022-07-20 09:52:47"
lastmod = "2022-07-20 09:59:24"
epistemic = "plant"
tags = ["cli","fish"]
+++
In software tools I gravitate towards simplicity, then ubiquity. The fish shell has those priorities in the perfect order. But if you use it for working with Python virtualenvs, you'll find it's not ready. This is probably because virtualenv has poor support for anything but bash, but I don't know for sure.

The problem lies in the activation script. While you're average shell can just run `venv/bin/activate` and you're in, fish has two obstacles. First, the activate script isn't written in fish script. Second, the activate script doesn't modify the shell prompt.

A solution is to use virtualfish. Functionally, it's great. My only qualm is that virtualenvs are typically placed near the python code, but this module puts them in the user's home directory.

[VirtualFish Installation](https://virtualfish.readthedocs.io/en/latest/install.html#customizing-your-fish-prompt)
