+++
aliases = ["/gardens/personal-devops"]
author = "Alex Bilson"
date = "2021-03-10"
lastmod = "2022-01-25 10:36:02"
toc = true
epistemic = "evergreen"
+++
## The Journey Begins

When my raspberry pi arrived two years ago, I opened it with excited trepidation. Will I overcome the hurdles to self-hosting on an unfamiliar architecture and operating system? How performant will it be? What tools will I learn, or give up, to achieve my goals?

Once I had figured out the {{< backref src="/gardens/technology/steps-to-self-hosting" >}}, I felt confident hosting my own static blog. But it wasn't long before I began to dream of more. What if I could publish content to my static site from anywhere, and not only when I have network access? What if I want to integrate other services into my website, like a daily memorial of covid-related deaths in Cook County?

My first project after a working static site was to create a way to continuously publish. The coolest writing tools already have this functionality, so why can't I? My site's content is stored on Github, so it was fairly simple to use Github hooks, a webhook server, and a little client-side JavaScript to accomplish the same workflow. To improve security I ended up writing my own publishing server, but that's another story.

Still amazed that my publishing server works, I discovered this amazing data publishing tool called [datasette](https://github.com/simonw/datasette). Simon's examples got me dreaming of what I might do with a plug-n-play sqlite web interface, and I landed on displaying the daily covid-related deaths in Cook county (see {{< backref src="/plants/technology/data-visualization" >}}).

Somewhere between running my static site and creating a publishing server, I became nervous that all my tinkering would be demolished if my raspberry pi's SD card were corrupted. Time for {{< backref src="/plants/technology/learning-ansible" >}}!

Ansible turned out to be a time-saver. Not only did it give me disaster recovery, it also simplified standardization. For example, when I began to secure my services by running each as a distinct non-root user, the boilerplate user/group setup was simple and replicable via Ansible. It would have been more error prone and annoying if I'd done each service by hand, one at a time.

> If you're going to use Ansible to manage server configurations (which you totally should), don't mix-and-match with direct updates. As soon as you start making configurations directly on the server you become nervous about using automated configuration lest you accidentally overwrite your work! #lessonslearned

Now I have a few services running on my raspberry pi, but {{< acronym FOMO "Fear Of Missing Out" >}} is rising - is it time to go full container, kubernetes style? There's even [k3s](https://k3s.io/), a light-weight version of kubernetes that's designed for my raspberry pi!

## Container CI/CD Diversion

I manage my services with supervisord and enjoy the freedom to configure each aspect. I can limit system access, configure logs, and manage ingress without much trouble. Linux services enjoy inherent stability and add little overhead. I've used Ansible to deploy my growing configurations so the fear that I'll lose it all is reduced. But it's still a lot to review and configure. Wouldn't it be cool if I were running k3s with a cluster of containerized services instead of a "normal" machine with boring old systemd services?

When I began to chart this course, I uncovered an assumption in my existing workflow. I conceptualized my existing setup as a single production deployment with several services, but when I tried to map each service to a container it became obvious that there are two distinct service types - {{< acronym "CI/CD" "Continuous Integration / Continuous Deployment" >}} and production. For example, I imagined running my webhook service in a single container but soon realized that every time the service created a new page on my static site it would need to generate a new static site container and replace it in the same cluster. A container that replaces other containers in the same cluster? This smells fishy to me.

So I pondered alternatives and came to the conclusion that larger kubernetes instances likely run separate clusters for CI/CD and production. So it seems I'd have to run two clusters to make my workflow happen - not awesome. All this to replace a small set of tiny Linux services that haven't given me a hiccup in over a year?

Of course, if this were a twenty person development team working on a dozen microservices, the ability to generate and test new container images by pushing a Git branch is stellar. The chances for error would increase if each developer independently updated their microservice configurations live on the servers like I do with my raspberry pi. No mystery why a company can justify running multiple CI/CD kubernetes clusters. However, I'm not a twenty person team so, as cool as running a CI/CD kubernetes cluster on my raspberry pi would be, I can't justify the cost. But I still want to understand how production kubernetes works, so I shelved this diversion for a bit.

> I did look into creating a lightweight CI/CD cluster with [podman](https://podman.io/), [buildah](https://buildah.io/) and [skopeo](https://github.com/containers/skopeo). While this path has merit, I found that ARM support isn't complete enough for me to try these tools out without low-level troubleshooting. I'm just not committed enough to my pet projects to spend hours digging through errors.

## Another Day, Another Perspective

When I came back to the problem, I decided to create a network diagram to describe my existing infrastructure and a second diagram to describe my proposed kubernetes infrastructure. By starting with what I had, I soon realized that I didn't need to think about deploying new containers on a regular basis if I treated the static content like a database. When Markdown files are added, if they're shared via persistent storage instead of inside containers, all my container deployment issues went away!

A diagram is worth ten thousand words, so here's what I've modeled. First, my existing architecture.

{{< image "/data/svg/bare-metal_deployment.svg" "diagram" >}}

Notice how simple it is. When I push an update to my site, my webhook service pulls the update and compiles the site for nginx to serve. If I send an update with my publishing service, it stores the new file in my content repo, pushes a change, and the same webhook process fires. To load data onto my static site I run a datasette instance. Simple and effective.

Then, my proposed kubernetes cluster architecture.

{{< image "/data/svg/k3s_deployment.svg" "diagram" >}}

I've split the deployment into two because my site does not depend upon the data server to operate, but it does depend upon my build and publish server to manage my own CD process. I've also moved from a model where every time I publish it stores a new file, pushes it to my repo, and rebuilds to a scheduled build. I've kept the publish and build processes separate so that I can still support either or both options, but I've found that I don't care that much about seeing what I publish immediately render on my site.

My hope is to add little complexity by switching to kubernetes while enabling me to deploy updates to any part of the system by swapping in a new container image, but I'm still learning kubernetes and haven't actually tried this (yet). Feedback is welcome 😁.

## Conclusion

Here's where I've landed, at least for today.

The state of container CI/CD at this time requires a dedicated node, and I'm not ready to buy another raspberry pi to make that happen. But I don't need container CI/CD anyways, just a means to back up my Markdown content, so who cares? I can run the whole shebang on my raspberry pi with no need for regular container deployments AND get practice running k3s. Win/win, don't-cha think?

## Revisited

My self-hosting journey has taken many turns since it began in 2019.

My first foray was little more than an Nginx proxy serving up Hugo-generated HTML files. As needs arose I began to add web services. Automated deployments with a webhook server and mobile publishing with a custom publishing service were two of the first. At the time I wrote this original post, I was also running a data-publishing service called {{< outref name="datasette" src="https://github.com/simonw/datasette" >}}, deploying server updates with Ansible, and managing all processes with supervisord.

I thought the natural progression was towards Kubernetes, so I attempted to make the transition to {{< outref name=k3s src="https://k3s.io/" >}}. When I drafted a network diagram, however, I discovered that the CI/CD process I wanted to implement to deploy my Kube nodes would require a second machine, and I didn't want to spend the money or manage the extra configuration. I had attempted an intermediary Podman installation but had run into difficulties configuring Debian Buster since it's not officially supported. But I didn't get very far until I also experienced trouble with k3s and, unwilling to let my site languish for weeks while I tried to implement a deployment process I didn't need, I abandoned the project. Fortunately, after bit of tinkering I _did_ get Podman working. And it's been a dream come true.

## The Podman Era

{{< image "/data/img/podman-services.png" "services" >}}

One of the intermediary transitions I needed to move my services to Kubernetes was to containerize them. I had a few in Docker containers for local development, so it wouldn't take much, but running my production services as containers was a new experience. And it's been such a great experience.

Podman has been a pleasure to operate for three reasons.

- Instead of relying on a mysterious Docker daemon to operate my containers, Podman let me convert each container unit into a systemd service so I can leverage the stability and support of Debian's process management. Systemd services can be enabled to launch on startup and log output to the system journal. It just feels more, integrated.
- Docker didn't have native pod support. I couldn't launch a web service plus database as a single unit without a bit of finagling, or by using Docker Compose. Podman has pods that work identically to pods in Kubernetes and can be operated as a unit with, you guessed it, the systemd service architecture.

- Debian Bullseye is now a stable release, which means that Podman is now officially supported! I don't know if Docker was ever _officially_ supported, though I did see installation tutorials.

## Updated Architecture

{{< image "/data/svg/podman_arch.svg" "podman architecture" >}}

## Conclusion

Thanks to {{< outref name="Jeff Geerling's" src="https://www.jeffgeerling.com/" >}} invaluable YouTube {{< outref name="Cluster Series - Part 1" src="https://youtu.be/kgVz4-SEhbE" >}}, it's evident that I need at least three nodes are necessary before Kubernetes is a worthwhile option. One node is dedicated to operating the cluster and offers little but resource consumption if it's the only node. Two nodes allow for only a single node for the master node to manage - again offering little and supplying no opportunity for Kubernete's container distribution logic. Two nodes for the master node to manage does give Kubernetes the chance to select the best node for a given pod, to have a backup for failed pods, and starts to become tedious to operate independently (barely).

{{< notice >}}
...at least three nodes are necessary before Kubernetes is a worthwhile option.
{{< /notice >}}

Is it time to move to Kubernetes? There's never been a better opportunity to make the attempt but, for my purposes, Podman may be the sweet spot. There's nothing k3s offers that my little Podman cluster can't already do with fewer resources. But with Debian Bullseye moving me closer to a fully supported OS, I might have to try anyways, if only to learn.
