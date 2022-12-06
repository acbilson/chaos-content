+++
author = "Alex Bilson"
date = "2022-12-06 10:12:12"
lastmod = "2022-12-06 10:46:40"
epistemic = "sprout"
tags = ["web-component","javascript","html","dom"]
title = "Prefer Light DOM to Shadow DOM"
+++
While there are legitimate uses for Shadow {{< acronym DOM "Document Object Model" >}}, light DOM ought to be preferred until it is _absolutely necessary_ to place your component's element tree outside the main document.

One common use-case which does not require shadow DOM is a custom element that does something special with events.

## Light DOM Use-Case

Let's say you have a custom element called `<chaos-filter>`. It has a form with a `<select>` dropdown and a filter `<button>` which applies the selected option to a list of children. Your custom element just needs to listen to the filter button click and apply a filter to the list. It's a fantastic custom element use-case because it combines dependent structure and logic in one reusable element.

The `<select>` and `<button>` are likely to have global design criteria and should be accessible to the rest of the DOM. Plus, you don't know when events from your element could be useful elsewhere in the DOM tree. For example, one might want to display a notice about the applied filter elsewhere on the page. Thus, your `<chaos-filter>` should be in the light DOM.

{{< notice type=quote name="Joe Gregorio" src="https://bitworking.org/news/2019/07/looking-back-on-five-years-of-web-components/" >}}
I’ve come to think of the Light DOM children of elements as part of their public API that goes along with the attributes, properties, and events that make up the ‘normal’ programming surface of an element.
{{< /notice >}}

> Yes, you could use a `<slot>` for the children. But you don't have to. And now you've got global CSS for your slot content and private CSS for your component. Unnecessary mess.

Sometimes, however, you're building a widget that does need full encapsulation.

## Shadow DOM Use-Case

Now you've built another custom element, let's call it `<chaos-date-picker>`. It has a four-level div structure inside to render all the dates, the selection, correct positioning, etc. The nested `<div>` and `<span>` elements have little inherent semantic meaning; only what you've supplied with ids and classes.

Access to the internal structure of your date picker is rarely necessary or desirable. Custom styling will need extra consideration for the effect it has on other parts of the component's tree structure. You can and should build a more user-friendly styling surface with attributes.

{{< notice >}}
If your custom element composes a semantically meaningful tree, it should probably be in light DOM.
{{< /notice >}}
