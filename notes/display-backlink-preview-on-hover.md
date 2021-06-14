+++
author = "Alex Bilson"
comments = false
date = "2021-06-11T20:43:55"
tags = ["snippet","javascript","software","backlink","microformat","indieweb"]
title = "Display Backlink Preview on Hover"
+++
The combination of microformat2 [h-entry](http://microformats.org/wiki/h-entry) and backlinks is potent. Each note page that's marked up with a `e-content` microformat2 property can be retrieved via `fetch()` and displayed as a preview elsewhere on the site. No database required; the website _is_ the API. Here's a JavaScript snippet to show how it works by reading only the first paragraph from the other page.

{{< highlight js >}}

// generate the popup element
function createPopup(offsetTop, offsetLeft, contentElement) {

  const popup = document.createElement('div');
  popup.id = 'popup';
  popup.classList.add('backlink-popup');
  popup.style.top = `${offsetTop + 20}px`;
  popup.style.left = `${offsetLeft}px`;
  popup.appendChild(contentElement);

  return popup;
}

// returns the child elements of the other page's e-content element or a missing message
function parsePageContent(text) {

  var bodyDOM = new DOMParser().parseFromString(text, 'text/html');
  const contentElement = bodyDOM.querySelector('div.e-content');

  if (contentElement) {
    return contentElement.firstElementChild;
  } else {
    const missing = document.createElement('p');
    missing.text = 'no content found';
    return missing;
  }
}

// adds a popup containing the element children of the internal link's e-content element
function showBacklink(event) {

  const link = event.target;

  fetch(link.href).then( (resp) => {
    if (resp.status === 200) {
      return resp.text();
    } else {
      return null;
    }
  }).then( (text) => {
    if (text === null) { console.log('no content found'); }
    const contentElement = parsePageContent(text);
    const popup = createPopup(link.offsetTop, link.offsetLeft, contentElement);
    document.body.appendChild(popup);
  });
}

// removes popup from the DOM
function hideBacklink(event) {
  const popup = document.getElementById('popup');
  document.body.removeChild(popup);
}

// adds backlink events to all links marked internal
const internalLinks = document.querySelectorAll('a.internal');

internalLinks.forEach(function(link) {
  link.addEventListener('mouseover', showBacklink);
  link.addEventListener('mouseout', hideBacklink);
});

{{< / highlight >}}

> The way I'm showing/hiding the popup is fragile. I'd recommend loading all popup content onto the page as hidden divs, then unhide the hovered content. This way there will only be one fetch per link and the style toggle will not likely hang if you drag your cursor over multiple links too quickly.

