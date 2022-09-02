+++
author = "Alex Bilson"
date = "2022-09-01 14:07:24"
lastmod = "2022-09-01 14:54:01"
epistemic = "plant"
tags = ["snippet","bundle","javascript","typescript","web-component"]
+++
I've successfully published hundreds of articles using only a static site generator. Sometimes though, I itch to create reactive elements like I build at work with Angular. What is a developer to do?

My needs are tiny, so here is a tiny way to get started.

{{< notice >}}
My intended goal is to supply a small number of custom web components for most pages on my website that I can inject via Hugo shortcodes. If your needs are greater than this, you'll probably need more than I suggest.
{{< /notice >}}

### Steps

Let's go through the list of steps to get a Hugo site running, since that's what I've got. I don't think it'll take much to convert this to other SSGs though.

#### Step 1: Choose A Web Component Framework

Yes, you don't _have_ to since web components are part of all modern browsers. But the API is more verbose and unintuitive if you're used to Angular or React. After trying Svelte and Preact, I decided these tools weren't made to deploy web components in the way I envisioned, where everything simply "works" by importing a script and using my custom element tag. So I opted for another framework I'd tried in the past, [Lit](https://lit.dev).

#### Step 2: Choose A Bundler

I'm decent with JavaScript, but I prefer TypeScript when I can. TypeScript creates a wrinkle in the process though, it needs a bundler. I didn't want to use a heavy-configuration tool like webpack. A little research unearthed [microbundle](https://github.com/developit/microbundle). Perfect!

#### Step 3: Install Requirements

Now that we've chosen our tools, let's install them. You'll need `npm`. Copy this `package.json` file to a folder near your site's content (or not, it's up to you). Then run `npm install`.

{{< highlight json >}}
{
	"name": "app",
	"type": "module",
	"source": "src/app.ts",
	"exports": {
		"default": "./dist/app.modern.js"
	},
	"scripts": {
		"build": "microbundle",
		"dev": "microbundle watch"
	},
	"devDependencies": {
		"lit": "^2.3.1",
		"lit-html": "^2.3.1",
		"microbundle": "^0.15.1"
	}
}
{{< /highlight >}}

#### Step 4: Create Your Web Component

That was quick! Now let's write our first basic web component.

{{< highlight js >}}
import {LitElement, html} from 'lit';
import {customElement} from 'lit/decorators.js';

@customElement('my-element')
class MyElement extends LitElement {

	render() {
		return html`
			<p>Hello world! From my-element</p>
		`;

	}
}
{{< /highlight >}}

Not much to it is there? It currently doesn't keep any state, but only renders a paragraph.

#### Step 5: Build Your Bundle

You've got a couple options here but, for maximum simplicity, I've chosen to bundle the entire Lit and Lit-Element modules along with my component. If that doesn't appeal to you, feel free to add them from a separate source (a CDN perhaps).

How do we bundle them all together? Well, `microbundle` assumes that anything marked as a developer dependency should be included in the final bundle. If you've used my `package.json` file you're good to go. If not, please move the `lit` and `lit-html` dependencies under `devDependencies`. Now run the following command:

{{< highlight sh >}}npm run build{{< /highlight >}}

#### Step 5.1: Woops, Experimental Decorators!

If you finished step 5 and got some error about decorators, we're on it! It's a TypeScript thing I guess. Maybe by the time you use these steps it won't happen, but if it does, you'll need this `tsconfig.json` file.

{{< highlight json >}}
{
  "compilerOptions": {
    "experimentalDecorators": true
  },
  "exclude": [
    "node_modules",
    "dist"
  ]
}
{{< /highlight >}}

#### Step 6: Deploy Your Bundle

There's probably a better way to do this, but I'm just copying my file to the assets folder here: `hugo_root/assets/js/app/app.modern.js`.

You'll notice I'm using the "modern" distribution. You can choose any of the `microbundle` outputs, but I'm not trying to maintain backward compatibility on my site. I'd prefer to use tactics like progressive enhancement to handle circumstances where user browsers don't load my web component.

#### Step 7: Embed Your Bundle

Last step! I hope it hasn't been too arduous.

Since we're bundling all dependencies into one file, only one line of HTML needs adding to whatever page we want to use our web component.

{{< highlight html >}}
<script type="module" src="js/app/app.modern.js"></script>
{{< /highlight >}}

Well, that's mostly right. I'm using Hugo's `resources.Get` for my own implementation, but you get the idea. Don't forget the `type=module` attribute or you'll get errors about `import` statements.

Now put your component wherever you want on the HTML page. It should look like this:

{{< highlight html >}}
<my-element>It didn't work!</my-element>
{{< /highlight >}}

{{< notice >}}
You might notice I've put "It didn't work!" inside the element. This is just for testing. Since the web component doesn't handle any internal content, this text will only display if our web component is NOT WORKING.
{{< /notice >}}
