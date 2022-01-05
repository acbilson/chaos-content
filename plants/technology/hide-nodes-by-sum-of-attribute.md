+++
author = "Alex Bilson"
date = "2021-12-30T14:46:02.429357"
lastmod = "2022-01-05 09:03:53"
epistemic = "plant"
tags = ["snippet","javascript"]
+++
My tags list has 195 tags with a single entry. One of the primary purposes for tags is to compare writing with the same tag to discover similarities, so these entries achieve little.

A simple button to the tags list with the following JavaScript will hide all tags with a plant and stone sum of less than two.

{{< highlight js >}}

document.querySelectorAll("div.terms-page__terms>div").forEach((el) => {
    var txt = el.childNodes[0].childNodes[1].innerText;
    var txtSum = txt.slice(1,-1).split('/').reduce((p,c) => Number(p) + Number(c));
    if (txtSum < 2) { el.hidden = true; }
})

{{< /highlight >}}
