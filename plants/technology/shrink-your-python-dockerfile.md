+++
author = "Alex Bilson"
date = "2021-12-06"
lastmod = "2023-02-22 15:22:34"
epistemic = "plant"
tags = ["docker","python"]
+++
A discovery I made when moving services to Docker containers was the sheer size of a Python container! If you only pull down the latest Python base image and install your dependencies, your image can be upwards of 200Mb. Fortunately, there's at least one multi-stage Dockerfile build tip that can save you lots of space.

### Local Package Install

The command `pip install -r requirements` installs your package dependencies under the root python library. This location makes it difficult to port these dependencies to another build layer, so we need to install the dependencies in the current user's local folder instead. Simple enough, the command changes to `pip install --user -r requirements`.

With dependencies installed under the local folder, copy over only your dependencies into a fresh Python image with a couple lines in your Dockerfile like this

{{< highlight sh >}}
FROM python:3.9.7-alpine3.12 as base
COPY --from=build /root/.local /root/.local
{{< /highlight >}}

{{< notice type=warning >}}
 This assumes the working user is root. If you're using a non-root user (a better security practice), substitute that user's home directory.
{{< /notice >}}

TODO: find out if [this article](https://cr0hn.medium.com/python-docker-images-in-less-than-50mb-8acc6ed776ec) can add even more savings.
