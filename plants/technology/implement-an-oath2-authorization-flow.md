+++
author = "Alex Bilson"
tags = [ "snippet", "oath2", "authentication", "mastodon",]
epistemic = "sprout"
date = "2022-12-3"
lastmod = "2022-12-03T15:00:47.758Z"
+++
TODO: finish this article

Add disclaimer about my example: may not need private server, etc.

## Steps

- GET authorization route from service with redirect to current page
- open route in new window (snippet, link to article about enabling popups on iOS)
- gather code from redirected url on same page
- send code to origin window and close new window. (snippet)
- send code to my server’s token endpoint
- server requests token with code. Returns token to client in response (could do cookie here too, I think)
- store token for later API calls