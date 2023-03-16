+++
author = "Alex Bilson"
date = "2022-05-19 14:26:51"
lastmod = "2023-03-16 07:48:54"
epistemic = "plant"
tags = ["deployment","ansible"]
+++
Before I can run my Ansible scripts on a backup server there are a few preparations to get it ready to connect.

### Step 1: Add a user

I'll add a user for Ansible to use. This command will prompt for a name and password.

{{< highlight sh >}}
adduser abilson
{{< /highlight >}}

My user will need sudo permissions so Ansible can run root privileged commands like chown.

{{< highlight sh >}}
echo 'abilson ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/010_abilson_nopasswd
{{< /highlight >}}

The user should also be given an ssh key.

{{< highlight sh >}}
ssh-copy-id -i ~/.ssh/my_ssh_key backup_server
{{< /highlight >}}

For convenience I also add an entry to my SSH config.

{{< highlight txt >}}
Host backup
  HostName 137.220.61.222
  User abilson
  Port 22
  IdentityFile /Users/me/.ssh/my_ssh_key
{{< /highlight >}}

Test ssh access then lock down permissions in /etc/ssh/sshd_config to:

1. Enable public key authentication
2. Disable password authentication
3. Disable Root login

{{< highlight txt >}}
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
{{< /highlight >}}

### Step 2: Run Ansible

Ok, _now_ I can run my Ansible scripts to configure the web server and install my chaos suite.

### Step 3: Configure SSL Cert

I can't use HTTPS until I've added a certificate from my trusted certificate authority, Let's Encrypt. Configuration starts with certbot:

{{< highlight sh >}}
sudo certbot certonly --manual --preferred-challenge dns -d ofchaosandorder.com -d '*.ofchaosandorder.com'
{{< /highlight >}}

Follow the manual steps to create TXT records. You'll need one for each of the two entries. I use two entries because I use subdomains for my services and a bare URL for my site. If you're not using Namecheap, this can be easier, but alas. They do have 'cheap' in the name after all.

### Step 4: Add SSL to Nginx

Add the following lines to the root site's server block. If you don't have to do this manually, the python3-certbot-nginx package can do this for you.

{{< highlight sh >}}
ssl_certificate /etc/letsencrypt/live/ofchaosandorder.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/ofchaosandorder.com/privkey.pem;
{{< /highlight >}}
