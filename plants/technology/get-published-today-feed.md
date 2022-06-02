+++
author = "Alex Bilson"
date = "2022-05-20T20:55:54.661569"
lastmod = "2022-06-02 09:44:42"
epistemic = "plant"
tags = ["snippet","javascript","rss"]
+++
This JavaScript snippet reads multiple RSS/XML feeds on my site and adds a list of the items published on this day to the bottom of my page. I may use this in future to add a "Published On This Day" feed to my site, kind of like how OneDrive will pop up a picture from last year.

{{< highlight js >}}
const parser = new DOMParser();
const today = new Date();
const lastYear = new Date(today.getFullYear() - 1, today.getMonth(), today.getDate());
const logsUri = this.location.href + 'logs/index.xml';
const plantsUri = this.location.href + 'plants/index.xml';

const copyrightEl = document.querySelector('div.copyright');
const lastYearEl = document.createElement('ul');

Promise.all([fetch(logsUri).then(x => x.text()), fetch(plantsUri).then(x => x.text())])
	.then(results => {
	const [logsTxt, plantsTxt] = results;
	const logsXml = parser.parseFromString(logsTxt, "text/xml");
	const plantsXml = parser.parseFromString(plantsTxt, "text/xml");

	const dateElements = [...logsXml.getElementsByTagName('pubDate'), ...plantsXml.getElementsByTagName('pubDate')];
	const onThisDay = dateElements
		.filter(el => {
		const elDate = new Date(el.innerHTML);
		return elDate.getMonth() === lastYear.getMonth() &&
				 elDate.getDate() === lastYear.getDate();
		}).map(el => el.parentNode);
	onThisDay.forEach(item => {
		const descriptionHtml = item.querySelector('description').firstChild.data;
		const childEl = document.createElement('li');
		childEl.innerHTML = descriptionHtml;
		lastYearEl.appendChild(childEl);
	});

	copyrightEl.parentNode.insertBefore(lastYearEl, copyrightEl);
});
{{< /highlight >}}
