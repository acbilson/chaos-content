+++
draft = true
category = "technology"
date = "2021-03-20"
title = "Personal DevOps"
description = "In which Alex explores the vast array of options for personal devops."
categories = ["technology"]
tags = ["consulting", "devops", "ci/cd"]
[featuredImage]
  large = ""
  small = ""
  alt   = ""
+++
When my raspberry pi arrived two years ago, I opened it with excited trepidation. Would I overcome the hurdles to self-hosting on an unfamiliar architecture and operating system? How performant will it be? What tools will I learn, or give up, to achieve my goals?

Once I had figured out the [steps to self-hosting](link here), I felt confident hosting my own static blog (that's where you're at right now). But it wasn't long before I began to dream of more. What if I could publish content to my static site from anywhere, and not only when I have network access? What if I want to integrate other services into my website, like a daily memorial of covid-related deaths in Cook County?

My first project after a working static site was to create a way to continuously publish. The coolest writing tools already have this functionality, so why can't I? My site's content is stored on Github, so it was fairly simple to use Github hooks, a niftly little webhook server, and a little client-side JavaScript to accomplish the same workflow. To improve security I ended up writing my own publishing server, but that's another story.

Still amazed that my publish server works, I discovered this amazing data publishing tool called datasette. The examples got me dreaming of what I might do with a plug-n-play sqlite web interface, and I landed on [displaying the daily covid-related deaths in Cook county](link here).

Somewhere between running my static site and creating a publishing server, I became nervous that all my tinkering would be demolished if my raspberry pi's SD card were corrupted. Time to learn Ansible!

Ansible turned out to be a time-saver. Not only did it give me disaster recovery, it also made the standardization of best practices simple. For example, when I began to secure my services by running each as a distinct non-root user, the boilerplate user/group setup was simple and replicable via Ansible. It would have been more error prone and annoying if I'd done each service by hand, one at a time.

> If you're going to use Ansible to manage server configurations (which you totally should), don't mix-and-match with direct updates. As soon as you start making configurations directly on the server you become nervous about using automated configuration lest you accidentally overwrite your work! #lessonslearned

Now I have a few services running on my raspberry pi, but FOMO is rising - is it time to go full container?

With my existing setup, managed at a service level by systemd and supervisord for certain multi-process services, I have huge freedom to configure each aspect. I can limit system access, configure logs, and manage ingress without much trouble. Linux services enjoy inherent stability. I've used Ansible to deploy my growing configurations so the fear that I'll lose it all is reduced. But it's still a lot to review and configure. Wouldn't it be cool if I were running k3s with a cluster of services instead of a normal machine with supervisord?

When I began to imagine how this might work, I uncovered an assumption in my existing workflow. I was under the impression that my services might all be categorized as production services, but when I tried to map them to containers it became more obvious that there are two distinct services - CI/CD and production. For example, I imagined serving my webhook service in a single container but soon realized that when the service created a new page on my static site it would need to generate a new container and replace it in the same cluster. This smells fishy to me.

So I pondered alternatives and came to the conclusion that larger kubernetes instances likely run separate clusters for CI/CD and production. Now I'd have to run two clusters to make my workflow happen. All this to replace a small set of Linux services that haven't given me a hiccup in over a year?

Of course, if this were a twenty person development team working on a dozen microservices, the ability to generate and test new container images by pushing a Git branch is stellar. The chances for error would increase with each developer independently updating their microservice configurations live on the servers. That's why a company could justify running multiple CI/CD kubernetes clusters. I'm not a twenty person team and can't justify running even one CI/CD kubernetes cluster on a raspberry pi, but I still want to understand how production kubernetes works.

> I did look into creating a lightweight CI/CD cluster with podman, buildah and skopeo. While this path has merit, I found that ARM support isn't complete enough for me to try these tools out without a lot of low-level troubleshooting. I'm just not committed enough to my pet projects to spend hours digging through errors.

Here's where I've landed, at least for today. The state of CI/CD at this time requires a dedicated node, and I'm not ready to buy another raspberry pi to make that happen. But I already have a stable ~CI/~CD system that uses minimal resources. I was able to install podman from the kubic repository, which means that I could now integrate podman commands into my existing system to replace direct builds. So I'll update my tiny CD server to execute image builds and learn how to use Podman pods to update my containers. In the process I may want to run my own [image registry](https://hub.docker.com/_/registry/). The registry container would naturally fit into a CI/CD pod, but I may run it within my production pod to minimize complexity. When I have a running production pod, I can then consider moving to k3s. Podman pods can be managed with Kubernetes config files, so I'll be 90% of the way there.
