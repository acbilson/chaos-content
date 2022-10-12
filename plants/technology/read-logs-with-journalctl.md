+++
author = "Alex Bilson"
date = "2022-05-20 12:37:04"
lastmod = "2022-10-12 15:10:55"
epistemic = "plant"
tags = ["cli","logs","journalctl","debug"]
+++
`journalctl` is one of the best tools I've learned to use often when debugging my set of chaos services running Podman via systemd on my Raspberry Pi. Lots still to learn, but here are a few tidbits.

## Find out what your options are

If you print out the journalctl results with no filters it can be _way_ overwhelming. But figuring out what field to filter can seem impossible. For example, specific services will include their own fields. How are you to figure out what they are?

Try this nifty trick. It'll print out every field for the last log. Should give you a baseline, and find a more specific log entry to get program-specific fields.

{{< highlight sh >}}
journalctl -n 1 -o json-pretty
{{< /highlight >}}

When you have a promising field, run the next query to learn every possible value of that field in the journal. Try the SYSLOG_IDENTIFIER!

{{< highlight sh >}}
sudo journalctl -F SYSLOG_IDENTIFIER
{{< /highlight >}}


## Filter by your systemd unit file

It's common to need only the results from a single systemd unit file. Gotcha covered. I also like to see most recent at the top, which is what `--reverse` is doing.

{{< highlight sh >}}
journalctl --reverse -n 100 -u container-micropub.service
{{< /highlight >}}

## Print a field to file

Sometimes you want to parse the results in your own text editor. This is a query I ran to get a bunch of UFW block messages so I could look more closely at them.

{{< highlight sh >}}
journalctl --since yesterday _TRANSPORT=kernel --output-fields=MESSAGE -o cat > ufw.log
{{< /highlight >}}

## Show only Podman logs for a systemd unit file

If you're running nonroot Podman containers, you can still use journalctl. Here's a query that will get your user's logs. It'll even filter out the systemd logs so you can just see what the container has been outputting.

{{< highlight sh >}}
journalctl --user -n 1000 --reverse -u container-webhook SYSLOG_IDENTIFIER=podman
{{< /highlight >}}

## Additional Resources

- [Sematext JournalD Logging Tutorial](https://sematext.com/blog/journald-logging-tutorial/)
