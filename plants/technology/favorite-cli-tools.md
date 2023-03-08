+++
author = "Alex Bilson"
date = "2021-08-06"
lastmod = "2023-03-08 08:16:41"
epistemic = "plant"
tags = ["cli"]
title = "Favorite CLI Tools"
+++
Inspired by Ibraheem Ahmed's [modern Unix commands](https://github.com/ibraheemdev/modern-unix), here's a list of my favorite CLI tools. There's a lot of cross-over with Ibraheem's list.

## [Bat](https://github.com/sharkdp/bat)

While navigating the file system I often want to look at the contents of a file but don't need to open it in an editor. The traditional tool `cat` achieves this by printing the contents to the console. `bat` improves the experience with line numbers, spacing, syntax highlighting and even git symbols.

The other common use of `cat` is to pipe the output of multiple files into other commands, like performing a text search across multiple log files. Perhaps this was the original, since 'cat' is probably short for 'concatenate'. `bat` recognizes when it's used in a pipe and responds the same way (though to be honest, I end up using `cat` for this scenario out of habit).

## [Exa](https://github.com/ogham/exa)

The first command you'll learn in a terminal is `ls` (or perhaps `cd`). The default output gives the name of files and directories at your current location. Think of `ls` as short for 'list'.

I don't find the default output helpful, so I always alias `ls` with the `-l` flag. The original output is useful for passing file names into a pipe, but I use this command to review the contents of my location. The `l` flag gives me timestamps, permissions, and more. `exa` does the same thing (once you alias it to `exa -l`), only with colors!

## [Ripgrep](https://github.com/BurntSushi/ripgrep)

Nearly every day I ask the question, "does a certain text exist in this or these files?" The magic of `grep` allows me to find out without having to open and scan every file. It can use full regular expressions to find complex text in relation to other text, can look recursively through directories, and ignore certain types of files. If you've used it for a little while and for more than a simple word search, it gets clunky. `ripgrep` brings sensible defaults, highlighted output, and automatic ignore for the .git folder.

## Git

Version control is one of those ideas which, once you begin using it, you want for _everything_. I've typed the common git commands so often that, if you watched my hands closely while I slept–which would be weird, please don't–my fingers would be moving to the pattern of `git status`. This is how I keep a history of anything with text: this post, the theme that displays it, the server code that publishes it. Everything.

## [Justfile](https://github.com/BurntSushi/ripgrep)

This blurs the line a little since Justfile is a command line runner. But when I first realized that I could stop hacking Makefile to work with my Python and C# projects, I was so happy. Like many others on this list which bring modern defaults to long-standing tools, Justfile adds all the right options without being unwieldy.

## [jq](https://github.com/stedolan/jq)

This tool has simplified my work with JSON numerous times. It's such a great compliment to tools like `curl`, allowing me to parse a JSON response body into colorful, indented beauty. It's saved me multiple times from entering invalid JSON by trying to parse the string first. And it's helped me debug really huge JSON results by smartly navigating the JSON tree.
