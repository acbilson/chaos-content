+++
author = "Alex Bilson"
date = "2022-09-14 15:08:59"
lastmod = "2023-03-16 07:49:31"
epistemic = "plant"
tags = ["npm","deployment","authentication","web-component"]
+++
I wanted to share a set of authentication web components from chaos-auth so I could use them in my website’s static build. Keeping these components close to the authentication server lets me keep the responsibilities close, since these components will be heavily tied to my authentication implementation.

## Step 1: Publish Locally

To publish a package you’ll simply need a name and version value in your `package.json` file. Here’s a paired-down example:

{{< highlight json >}}
{
	"name": "chaos-auth",
	"version": "0.0.1",
	"type": "module",
	},
	"dependencies": {
		"lit": "^2.3.1",
		"lit-html": "^2.3.1"
	}
}
{{< /highlight >}}

With your `package.json` ready, publish your package to a local directory with:

{{< highlight sh >}}
npm pack --pack-destination ~/packages/
{{< /highlight >}}

## Step 2: Prepare For Installation

The package is a compressed tar file, but we’ll need to uncompress it for `npm` to correctly install inside another project. Simple enough:

{{< highlight sh >}}
gunzip ~/packages/chaos-auth-0.0.1.tgz
{{< /highlight >}}

{{< notice >}}
To my surprise, gunzip will extract where the file exists, not where the command is executed. No need to navigate to the directory for this command to work.
{{< /notice >}}

## Step 3: Install

The `npm cli` handles installation from a local directory very well. Simply run the following command in your consuming project:

{{< highlight sh >}}
npm install ~/packages/chaos-auth-0.0.1.tar
{{< /highlight >}}

Voila!

TODO: There’s a way to instantiate a local package repository with `npm link` which would allow me to easily pull multiple packages or updates, but I didn’t get it working right away, so I shifted to this slightly more tedious way.
