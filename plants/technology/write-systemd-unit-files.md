+++
author = "Alex Bilson"
date = "2022-05-12 11:10:31"
lastmod = "2023-03-16 12:06:19"
epistemic = "evergreen"
tags = ["systemd","podman","kubernetes"]
+++
I don't fully understand it's history, but there was a wave of conflict when Debian choose to standardize service initialization on systemd. I'm not here to argue for or against it; only to suggest that, if you're running a Linux machine that uses this as its core initialization, you'll benefit from using the same. Don't tack on another service manager if you can help it because, as I argue elsewhere, it's best to {{< backref src="/plants/technology/use-the-default-tools" >}}.

There are a couple excellent resources (besides the manual) for understanding systemd, one from [RedHat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/chap-managing_services_with_systemd) and another from [DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files).

I'm using systemd unit files to manage my Podman container processes. With systemd I can specify that a Podman container launches after my machine boots and whether it should launch before another container. The Podman CLI will even [produce the systemd unit files for you](https://docs.podman.io/en/latest/markdown/podman-generate-systemd.1.html)!

When you begin to use elemental Linux tools like systemd, you also lay a foundation of understanding all the tools which have been modeled after them. Read the systemd manual, then look at Kubernetes. Can you recognize the similarities? As I quoted in {{< backref src="/plants/technology/use-the-minimum-number-of-machines" >}}, Kubernetes is actually "a general-purpose cluster operating system kernel" ([ButtonDown](https://buttondown.email/nelhage/archive/two-reasons-kubernetes-is-so-complex/)). Using systemd is your precursor to running fleets of machines!
