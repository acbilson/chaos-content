+++
author = "Alex Bilson"
date = "2022-05-12 08:25:34"
lastmod = "2022-05-12 12:23:01"
epistemic = "plant"
tags = ["software","design"]
+++
I've read many articles from software developers about the value of learning multiple software languages. It may not be for everyone, but that's been a good path for me. I enjoy the challenge of learning new idioms, solving challenges in new ways, and reading the codebases of my favorite tools.

One problem these suggestions don't address is how to make choices after you've learned a half-dozen languages. If your employer runs a MSSQL/.NET/Angular stack then you may never need to decide if a new feature ought to be written in Rust or Python. You'd be ostracised for even making the suggestion, and you don't really want to support another language in production anyways.

When you're a consultant, or if you run your own software for other purposes, then language selection isn't about herd familiarity. You can start to consider how one language solves certain problems in a preferable way to another. You can compare deployment methodologies, performance statistics, and community support.

My mind is constantly evolving new thoughts about this subject as my familiarity with new languages grows, but here's a general rule of thumb for a few languages in which I have enough skill to run them in for my own production uses.

{{< notice type=warning >}}
You can make good arguments that every opinion I have about these languages is false. Usually I'm interested in a debate, but with this, just let me remain in ignorance. Please. However, if you have a favorite resource for building stuff in one of these languages, a book or a hyperlink, I would love to see that.
{{< /notice >}}

In container deployments, Golang is one of my favorites for the simple reason that it produces a single binary. There's a joy in building your project, dumping the exe into your final container stage, and pushing it to production that's not matched by others. Python can have complicated build dependencies and takes a bit of tweaking to keep build artifacts from bloating your final image. .NET Core suffers from a similar problem and, even though it's cross-platform, it can take some troubleshooting to make it function on a non-Microsoft architecture.

For CLIs, Rust wins the day. It's performant, safety-conscious, and has great package and documentation support for this use-case. The new wave of Unix commands-ripgrep, exa, and bat are a few of my favorites-are all written in Rust. There's good support in Python for CLIs, but you'll trade performance for scripting speed. And I don't know why you'd use .NET Core, though you might say that a PowerShell script qualifies (but then I'd have to throw in other shell programs and then we're off on a rabbit trail).

As a web service, Python shines. It has beginner-friendly support for everything you'll need to run a web service, from authentication to database management. Yes, .NET Core is the enterprise pick-I'm not suggesting businesses convert to Flask-but it takes an enterprise's resources to run the suckers. Golang also has good support for web services, but my bias is towards what I've used more frequently. It's helpful in Python that you've got Flask and Django to choose from; Golang doesn't seem to have a de-facto set of web frameworks to run yet and, if you're going to build your service without a framework, why not use Rust?

For previous thoughts on the topic also see {{< backref src="/plants/technology/choosing-a-tech-stack" >}}.
