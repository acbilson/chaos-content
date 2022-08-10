+++
author = "Alex Bilson"
date = "2022-08-10 07:49:26"
lastmod = "2022-08-10 07:49:53"
epistemic = "plant"
+++
To embed a OneDrive video I follow these steps:

## Step 1: Convert

If it is a MOV file (almost certainly), I'll need to convert it to MP4.

1. Download it to my laptop.
2. Convert the file to an MP4 with `ffmpeg`.

{{< highlight sh >}}
ffmpeg -i video.mov video.mp4
{{< /highlight >}}

TODO: May be worth considering compression options. Videos are humongous.

## Step 2: Upload

Upload the MP4 version to the Videos folder.

## Step 3: Embed

Thanks to [Omar](https://blog.omaration.com/embedding-videos-from-onedrive-into-your-blog/) I was able to figure out how to instruct the browser to download and play the video. I tried for a while to determine a stream option but nothing simple enough for self-hosting. [Owncast](https://owncast.online) was promising, but it wasn't clear how to stream a local file. The documentation is heavily skewed towards live video.

1. Replace 'embed' with 'download' in the OneDrive embed source URL.
2. Enter the following HTML (I use a shortcode):

{{< highlight html >}}
<video width="180" height="320" preload="metadata" controls>
  <source src="{{ $src}}" type="video/mp4">
  Your browser does not support the video tag.
</video>
{{< /highlight >}}

Of special note is the preload attribute. By default, the video will automatically download to the user's machine. My first video was 88Mb and, if I add a few more videos to my log this number could rise into the low Gb. I'm not gonna quietly download that much data without user consent. With this preload, only a little file metadata gets downloaded and the whole file only when a user clicks play.
