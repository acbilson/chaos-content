+++
author = "Alex Bilson"
date = "2021-06-11T20:05:50"
lastmod = "2022-10-12 14:31:10"
epistemic = "plant"
tags = ["snippet","javascript","html"]
+++
If you have a textarea where you enter content on a regular basis and want to implement snippets to help with certain constructs, this JavaScript example might be just your style. I may use it in my publishing service so I can easily enter Hugo shortcodes I've developed for my site.

{{< highlight js >}}
document.querySelector('textarea.content').addEventListener('keyup',
  (event) => {
    var txt = event.target.value;
    if (txt.length < 7) { return; }

    var lastWord = txt.split(' ')?.slice(-1)[0];

    if (lastWord === 'backref') {
      var replacedTxt = txt.slice(0, txt.length - 7).concat('{{</* backref "" "" */>}}');
      document.querySelector('textarea.content').value = replacedTxt;
    }
  });
{{< / highlight >}}

