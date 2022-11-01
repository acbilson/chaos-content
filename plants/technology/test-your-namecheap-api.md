+++
author = "Alex Bilson"
date = "2022-10-19 15:02:12"
lastmod = "2022-10-19 15:09:48"
epistemic = "plant"
tags = ["snippet","fish","namecheap","api","xml","dns","xh"]
+++
My Let's Encrypt SSL cert was accidentally expired. It'd been updated, but certbot created a new cert instead of updating the existing one. I don't know why. Anyways. This led me to explore how I might automate this and viola, trwnh has [done it](https://github.com/trwnh/namecheap)!

Before you plug in trwnh's Python script, however, maybe you just want to check out the API for yourself? Here's a fish script to do it.

{{< notice >}}
My script uses the nifty `xh` tool, but it wouldn't take much to downgrade it to `curl`.
{{< /notice >}}

{{< highlight sh >}}
#!/bin/fish
	set HOST "api.namecheap.com"
	set API_USER "my-user-name"
	set USER_NAME "my-user-name"
	set API_KEY "my-api-key"
	set IP_ADDR "my-ip-address"
	set SLD "domain"
	set TLD "com"
	set TTL "1799"

	# gets TXT records
	set BASE_REQUEST "https://$HOST/xml.response?ApiUser=$API_USER&ApiKey=$API_KEY&UserName=$USER_NAME&ClientIp=$IP_ADDR"

	xh -v "$BASE_REQUEST&Command=namecheap.domains.dns.getHosts&SLD=alexbilson&TLD=dev"
{{< /highlight >}}
