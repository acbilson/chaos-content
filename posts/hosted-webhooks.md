+++
category = "technology"
draft = true
categories = ["technology"]
date = "2020-09-23"
description = "In which Alex describes how to configure a personal webhook server."
tags = ["webhook", "hosting"]
title = "Hosting a Webhook Server"
[featuredImage]
  alt = ""
  large = ""
  small = ""
+++

If you've followed the steps to host your own website (TODO: add link), you've got a static site you can build and deploy to your local server. The process is simple and entirely within your control, but it does limit how you add posts to your website. If you just want to put a quick note up on your site, you've got to access the laptop from which you've built the website, write a new post, publish the update, and pull the update on our local server. That's a lot of steps, limited to your laptop. Can you add a note more easily?

Yes, you can! Enter the magic of webhooks.

A webhook is a simple server that accepts a payload and runs a command on the server when certain criteria are met. Many services use webhooks to allow developers to automate workflows. For example, Github offers webhooks to allow developers to kick off processes when command actions occur on their repository, such as sending an email when a pull request is submitted. But these services host their own webhook servers, which means we'll need to run the same technology on our local server to get the effect. (TODO: add link to adnan's webhook code)

If you're familiar with software, you know there are infinite other ways you could achieve this. You might build your own custom server that receives HTTP requests and executes functions on your server. There are probably already libraries to plug into your code that will manage common tasks like working with Git repos. This approach separates the server from the execution (which is in a script) and thus puts more burden on the admin to configure the file system instead of the developer to secure the server code.

First, let's discuss the bare minimum to get this setup working. It's nice to have the bare minimum so you can test it for yourself. Then we'll review security practices you'll want to implement before you start using this webhook.

sudo /usr/bin/webhook -hooks /var/www/alexbilson.dev/hooks/hooks.json -port 9732 -secure -cert /etc/letsencrypt/live/alexbilson.dev/fullchain.pem -key /etc/letsencrypt/live/alexbilson.dev/privkey.pem -verbose

upstream webhook_server {
  server 127.0.0.1:9732;
  keepalive 300;
}

limit_req_zone $request_uri zone=webhooklimit:10m rate 1r/s;

server {
  root /var/www/webhook_server

  location /hooks/ {
    proxy_pass http://webhook_server;
    # adds a limit to the number of requests to protect against spam and poorly configured webhook setups
    limit_req zone=webhooklimit burst=20;
  }


}
