+++
author = "Alex Bilson"
date = "2022-05-05 08:52:40"
lastmod = "2022-05-05 14:52:17"
epistemic = "sprout"
tags = ["deployment","ansible","redis","ci/cd","systemd","git"]
+++
Inspired by Christian Ştefănescu's brilliant design for a [Tiny CI System](https://www.0chris.com/tiny-ci-system.html) and motivated by the need to redeploy my entire web stack to a cloud server while we move, I've crafted my own minimal deployment system.
## Step 1: SSH Access

The first leg of the journey is to configure the server so that I can access it via SSH. I'll write down these steps again; for now just Google it.

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

A tiny-ci.sh script waits for updates to this queue and takes my job off the top. It gets the hash set values from Redis and starts work by changing to the repo's directory and checking out the revision. It runs a standardized just-cli task in the repo that kicks off a Podman build and restarts the systemd service.

When the image is built and the service bounced, the script updates the hash set with success and output values.

## Future Enhancements

I have to log into my remote server to check the deployment status. It'd be awesome if I could either:

a) host a lightweight web service that exposes job hash set data from Redis, or
b) get real-time notifications via a service like ntfy.sh
