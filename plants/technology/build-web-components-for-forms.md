+++
author = "Alex Bilson"
date = "2022-11-09 15:00:23"
lastmod = "2023-03-16 07:48:13"
epistemic = "plant"
tags = ["javascript","web-component","dom"]
+++
Forms are an obvious web component, but there are a few gotchas. Stephen solves a common problem when writing my own web components: [How to communicate with a form tag](https://dev.to/steveblue/form-associated-custom-elements-ftw-16bi). I'm curious to try his approach for myself.

I recently took a different approach: don't use the shadow DOM at all. Then the browser has no trouble connecting the `input` tags with the `form`. See my [chaos-filter](https://github.com/acbilson/chaos-theme/blob/5f63c5d35520df10347972c0357a3a0e6e44aaa0/app/components/filter/chaos-filter.ts), which is used on [this page]({{< ref "/plants" >}}).

The `chaos-filter` takes two slots, one a form to enter filter criteria, the other the content to be filtered. The web component itself delegates all formatting to global CSS properties so I don't need to reconfigure my web component anytime I want to change the look of my forms. Since it doesn't control the HTML layout, the web component must be made fairly generic. In my case, I've chosen to use data attributes to communicate linking information between the form inputs and the list to be filtered. I use `data-match` to indicate the name of the data attribute on the list which this input filters. For example, the `data-match="epistemic"` selector will filter list elements with a `data-epistemic="plant"` data attribute.

{{< highlight html >}}
<chaos-filter>
	<form slot="chaos-filter-form" action='https://alexbilson.dev/plants' method=get>
		<select data-match="epistemic">
			<option value="all">All</option>
			<option value="🌲">Evergreen 🌲</option>
			<option value="🪴">Plant 🪴</option>
			<option value="🌿">Sprout 🌿</option>
			<option value="🌱">Seedling 🌱</option>
		</select>
		<select data-match="folder">
			<option value="all">All</option>
			<option value="business">Business</option>
			<option value="culture">Culture</option>
			<option value="entrepreneurship">Entrepreneurship</option>
			<option value="faith">Faith</option>
			<option value="identity">Identity</option>
			<option value="leadership">Leadership</option>
			<option value="parenting">Parenting</option>
			<option value="technology">Technology</option>
			<option value="writing">Writing</option>
		</select>
		<input type="submit" value="Filter"/>
	</form>
	<!-- list of content here -->
</chaos-filter>
{{< /highlight >}}
