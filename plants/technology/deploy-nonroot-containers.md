+++
author = "Alex Bilson"
date = "2022-05-12 08:05:34"
lastmod = "2022-05-12 08:22:39"
epistemic = "sprout"
tags = ["podman","container","deployment"]
+++
One of the advantages touted by the Podman team over Docker is that you can choose to run containers as a user other than root. This is a security feature since, if a container were compromised by a malicious user and broke out of its container process, they would not have root privileges.

Actually running containers as non-root users, however, adds a surprising complexity. Most container images, including those I've created myself, were drafted in Docker. Since Docker runs as root, image developers don't need to consider how their container will operate if it's run by a non-root user.

Take the public Postgres image. It's well documented, continuously updated, and owned by the Postgres team. If you run it as a non-root user, however, it will crash, because in its instantiation it used `chown` to set ownership of the database folder. Fortunately, the team does offer a way to get around this problem by first generating the volume, then running root to change ownership, and then to finish configuring the volume. Maybe this ownership change is absolutely necessary, but it's a great example of a well-supported image that assumes root. How well do you think the other images will run as non-root?

{{< notice >}}
I discovered the chown issue running the latest Alpine Postgres image on May 11th, 2022. Future images may not feature the same work-around for non-root users or even need it. Check for yourself.
{{< /notice >}}

But it's not all complicated. One reason I've migrated to non-root containers is not because of security, but because of Git. More than one of my containers interacts with volumes that point to multiple Git repositories. When the containers run as root I have to jump through a few hoops to interact with the repo to keep ownership of the directory and files to my user. A root-owned obj or ref file has bitten me more than once.
