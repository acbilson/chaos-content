+++
author = "Alex Bilson"
date = "2023-04-05 11:27:58"
lastmod = "2023-04-05 14:10:12"
epistemic = "plant"
tags = ["gpg", "snippet", "cli", "security","encryption"]
+++
What if you need to send sensitive information to a person on the other side of the planet? What if they need to send something to you? Here we'll explore a simple options available to you.

## Email

Many email providers, such as Outlook and Gmail, encrypt sent email traffic by default. If both the sender and receiver email providers support TLS (Transport-Level Security), such as when one Outlook account emails another, then your message cannot be read if intercepted in-transit. If you trust your email provider to prevent access to your stored emails and the provider to which you've sent your message, that may be all the security you require.

## File Encryption

Perhaps you have sensitive data to transmit, but you can't control the transmission. Maybe it passes through multiple people, not all of whom you trust, the services you're using are suspect (see {{< backref src="/plants/technology/store-secret-text-in-the-cloud" >}} for a Dropbox example), or you have concerns about government interference. You'll want to secure the data itself via encryption.

I won't bore you with the numerous options and their pros/cons. Let's focus on one globally available and free option: key/asymmetric encryption with GnuPGP. To understand how this works, let's use a physical example.

You are sending a folder of sensitive information to your friend in a neighboring country. The folder travels through many postal hands and probably a government censor before it arrives. How do you get it there safely?

First, your friend mails you an empty safe to which only they know the combination. You place your sensitive information into the safe and lock it. Then you mail it back. If your friend needs to send you information, the same process happens in reverse.

{{< notice >}}
That's the gist, but GnuPGP adds one more level. It locks your sensitive information in a strongbox and sends the box's key in the safe. It sends a different key every time so that, even if one key were compromised it would only open one strongbox.
{{< /notice >}}

If you're familiar with Edward Snowden's story, this is how he sent sensitive information securely.

## Safety Concerns

Human error is the #1 threat to any secure system.

Your email may be encrypted in-transit but that doesn't prevent anyone who possesses the email from forwarding it to an unsecure location. Or from losing the phone that a copy of the email was stored on.

Likewise, file encryption is technically impenetrable, but people can be tricked. A stolen laptop compromises their private key (their combination), meaning that if you send data to them in their safe, someone else can open it. Likewise, you might receive a public key (safe) which, if you don't confirm that it's owned by your friend, it can be opened by the safe's true owner. Even worse, the true owner may take out your document, put it in your friend's safe, then pass it along so that no one suspects they were in the middle.

Yet, unless you're Edward Snowden, it's unlikely that you'll face the kind of targeted scrutiny that could compromise these essential security tools. By taking a few steps to prevent your private key from exposure and verifying that you have your friend's actual public key, you can confidently use this pattern with minimum effort. I don't say no effort–all security measures add friction–but little enough that they're not circumvented or forgotten by hasty humans.
