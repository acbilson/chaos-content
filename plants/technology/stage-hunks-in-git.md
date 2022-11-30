+++
author = "Alex Bilson"
tags = [ "git", "cli", "snippet",]
epistemic = "evergreen"
in-reply-to = "https://pawelgrzybek.com/git-tip-staging-hunk-of-code-via-command-line/"
date = "2022-11-30"
lastmod = "2022-11-30T17:48:03.918Z"
+++
Like [Pawel](https://pawelgrzybek.com/git-tip-staging-hunk-of-code-via-command-line/) I've been tethered to source control GUIs because I didn't know how to stage code hunks in the same file into different commits. But no longer!

The answer is so simple I can't believe I didn't search for it earlier.

{{< highlight sh >}}
git add -p name_of_file.txt
{{< /highlight >}}

How easy is that?