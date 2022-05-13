+++
author = "Alex Bilson"
date = "2022-05-05 08:52:40"
lastmod = "2022-05-13 11:00:51"
epistemic = "sprout"
title = "Build Your Own CI/CD Pipeline"
tags = ["deployment","ansible","redis","ci/cd","systemd","git"]
+++
Inspired by Christian Ştefănescu's brilliant design for a [Tiny CI System](https://www.0chris.com/tiny-ci-system.html) and motivated by the need to redeploy my entire web stack to a cloud server while we move, I've crafted my own minimal deployment system.

## Step 1: SSH Access

The first leg of the journey is to configure the server so that I can access it via SSH. These steps are common knowledge or a simple Google search so I won't reiterate.

## Step 2: Server Configuration

Now that I can use SSH I'm able to run my Ansible web/deployment server playbook. This playbook installs common admin programs, installs and configures my Nginx web server, and enables a firewall. It also installs podman, redis, and the glue scripts used for the CI/CD process, and the just cli from source.

> shoutout to Steve Owens for Ansible help with [Ansible Git Server Installation](https://opensource.com/article/17/8/ansible-environment-management).

## Step 3: Chaos Suite Installation

With the server configured, I run a second Ansible playbook to install all my chaos suite roles. This installs the proxy configuration and systemd files, adds a bare Git repo, and includes a post-receive hook to interact with the CI/CD redis service.

## Step 4: Add Git Remote

The final step is to configure my local repo to push to the new remote.

## How It Works

Let's say I'm working on my chaos-micropub service. I've finished a feature and want to deploy it to production, so I ~cut a release branch~ merge it into master. Then I push the changes to my CI/CD-enabled remote.

{{< highlight sh >}}
	git push pi master
{{< /highlight >}}

I get a message back that reads:

{{< notice >}}
	started job 0d313b1e-cca8-11ec-8c59-67453021b8a9 for chaos-micropub.
{{< /notice >}}

This job was added to a Redis queue with that UUID along with a few hash set values that are relevant to the build: ref, revision, and repo.

A tiny-ci.sh script waits for updates to this queue and takes my job off the top. It gets the hash set values from Redis and starts work by changing to the repo's directory and checking out the revision. It runs a standardized ci-build.sh bash script in the repo that kicks off a Podman build and restarts the systemd service.

When the image is built and the service bounced, the script updates the hash set with success and output values.

## A Note About Nonroot Containers

You can make all of these steps work with root containers, but you'll hit one snag when restarting a nonroot container that I haven't solved yet. If you're running any nonroot container with this method you'll have to manually redeploy the systemd service.

SystemD allows user-specific unit files which makes perfect sense to manage user-specific Podman containers. However, there's not presently a way to execute a systemctl command to restart your container from within a script. If you have a line in your bash script that looks like this:

{{< highlight sh >}}
	systemctl --user restart container-micropub`
{{< /highlight >}}

It will fail with a message about XDG_RUNTIME_DIR and DEBUS_SESSION_BUS_ADDRESS.

Some Googling revealed that you could run `loginctl enable-linger ${user}` to solve this, but it doesn't quite work for this scenario. Linger allows your user to launch the systemd unit file at boot if it's enabled. These errors are about something else.

From what I have been able to gather, the problem (if you want to call it that) is isolation and identity. SystemD maintains strict isolation between systemd services for the root user and those for a specific user. If you try to restart the systemd service from a script, it balks because it can't confirm that you're a "real" user (identity). However, if you try to impersonate a user with root to hack your way in, it will also fail. Although the systemd manual mentioned the `--machine` flag can allow root to run systemd services as another user, I was not able to get it to work.

So there's no way that I've found to run a user's systemd service except by logging in and doing it myself.

## Future Enhancements

I have to log into my remote server to check the deployment status. It'd be awesome if I could either:

a) host a lightweight web service that exposes job hash set data from Redis, and/or
b) get real-time notifications via a service like ntfy.sh
