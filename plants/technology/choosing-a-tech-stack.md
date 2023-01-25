+++
author = "Alex Bilson"
date = "2022-03-18"
lastmod = "2023-01-25 13:59:10"
epistemic = "plant"
tags = ["software", "optimization"]
+++
Software projects often get postponed while I decide on the tech stack. What kind of data storage? What programming language? Which framework, if any? My abundant choices sometimes paralyze me from actual building.

There are many stories one might approach software development, but one that I might benefit from goes, "code is cheap. Build it with what you know and, if it needs to be improved, rebuild it." Julia Evan's [three points from Ines Sombra](https://jvns.ca/blog/2017/01/24/choosing-the-best-thing/) explores this in the reverse; what motivates a system change:

1. It’s too expensive
2. It’s too difficult to operate (humans spend a ton of time worrying about it)
3. It’s not doing the job it’s supposed to

The chaos-micropub service I'm using to write this quip is written in Python with Flask. I went through a few iterations applying best practices, but the original code was quickly implemented because I'm already familiar with Python and the Flask micro-framework. My backlog's had a task to re-write it in Golang for weeks now, but it's hard to justify rebuilding what works fine. Original projects are often written in Python because it's so easy to build a rapid prototype. Maybe I should just stick with Python...

Javascript for the backend (Node.js) is, in my mind, identical to Python. It offers type flexibility and is a language I know equally well. Maybe the similarities are why I've never considered it for more than frontend development.

Then there's C#. I haven't built a personal project in the language in a while, but it's the most production-ready software I know. There's a reason many businesses use it for their production services (yes I know, there are glaring exceptions). I'm as comfortable in C# as I am in Python and appreciate the cross-over learning between private and public projects. But Visual Studio...

Also, Golang. While C# benefits from familiarity, I've found Golang to be easy to pick up. I'm not happy with the huge binaries that C# produces (though I could learn ways to minimize the size), whereas Golang produces tiny executables for minuscule container images. But I've never used the language in a business setting and feel unsure how strongly I want to become a knowledgable Golang developer. I think because, while Python and C# offer server-side HTML templating in Jinja and Razor, my searches into Golang's frameworks return lists of JSON APIs. I don't _want_ to build a SPA for everything.

Finally, Rust. In my mind, the ultimate optimization. While I might be able to write the fastest, most complete code in Rust, it'll also take me the longest and I can't depend upon a huge community to supply libraries for stuff I don't want to implement myself. So, yeah...

I feel the need to standardize on _something_. But what? I've started down the path of C# with Blazor Server, but I'm hesitant to put my hopes on a system that's not a decade or more old. I can't reliably expect Rust to handle the various needs I may have. Golang is an option, but I'm put off by the seeming lack of a suitable web framework. I'm probably being a little hard on the language since clearly it does have options (I use Hugo). Finally, I know that Python works for me but it doesn't feel production-grade for some reason.

Sigh. Well, thanks for listening. I'm going to finish this C# Blazor Server project and see where that leaves me I guess.

## Update

Even though I was able to produce a dotnet 6 Blazor Server app and run it in a production configuration locally on my Macbook Air, the compilation was so terrible to publish it on my Raspberry Pi that I'll have to abandon the project entirely for another language. Good riddance; while I don't mind using dotnet for work, it's really horrible to use for personal projects. It took me half a day just to configure the URL for my project because there are so many documented ways of doing it, with changes in the last three major versions of dotnet.

## Another Update

My programming language comprehension has drifted and it’s made language selection more complicated. I might choose to write a program in C#, TypeScript, Go, Rust, or Python (not to mention HTML/SCSS and T-SQL). Most enterprise software I’ve built is in C# and Typescript, with some SQL. My own services run either Python or Go.

The ambiguity regularly makes we long for standardization. I spent a little while last year attempting to build C# podman images but found the build resources were too high and the image size was bloated. So I decided against C# standardization.

I’m not sure about using Go as a standard. It’s similar enough to C# that I’m not sure it’s worth the effort, plus I’ve been swayed by Rust developers that Go may not be an optimal low-level language.

It’s becoming obvious that I won’t be able to land on a single language, but a pair might do. Rust/Python seem like a good match. Rust is exceptional for low-level programs like CLIs, parsers, etc. Python, on the other hand, is an invaluable high-level language for scripting and web services. Rust could operate as it’s own web service, so it’s not crucial to use both in the same container, but the option is there. I wonder though if there needs to be a third language?

The winners of the day are:

### Python

Python is a wonderful utility language. It's easy to launch web services and write scripts. It's straightforward to {{< backref src="/plants/technology/run-cli-commands-in-python" >}} and thus interoperable with Rustlang. With the type system in place, it gets a lot safer to write Python code too.

### Rust

Rust is the CLI builder. Most of my favorite CLI tools are written in this language, and it's the first one I'd choose for heavy work with local files, parsing, and analysis. One could do more with it, but it has the best support for this level IMO. Eventually, I might replace simpler Python services with Rustlang as a learning experience.

### TypeScript

Now that I've learned to {{< backref src="/plants/technology/embed-web-components-into-static-html" >}}, the potential for lightweight reactive components is high. I've enjoyed writing TypeScript for front-end work.
