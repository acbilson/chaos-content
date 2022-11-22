+++
author = "Alex Bilson"
date = "2022-11-17 12:39:27"
lastmod = "2022-11-22 07:39:44"
epistemic = "plant"
tags = ["javascript","typescript","testing","playwright","web-test-runner","web-components"]
+++
So you've created a native web component or two. How do you test them in popular browsers?

## Packages

There are a few tools to install.

First, [test-runner](https://modern-web.dev/docs/test-runner/overview/) to find and execute our tests. It comes with the [Mocha](https://mochajs.org) test framework by default, and we won't change it. Might as well install [chai](https://www.chaijs.com) too for test assertions and expectations.

Second, the browser runner [Playwright](https://playwright.dev) plugin. Playwright integrates headless browser testing into the mix, allowing us to run our code inside a browser context for every one of the major browser binaries (chromium, firefox, webkit).

And if your web components are TypeScript, we can install an [esbuild](https://esbuild.github.io) plugin to convert TypeScript modules into browser-readable code as-needed.

{{< highlight sh >}}
npm install --save-dev @web/test-runner @esm-bundle/chai @web/test-runner-playwright
npm install --save-dev @web/dev-server-esbuild
{{< /highlight >}}

## Configuration

There's always configuration (sigh). I'll give you the minimal amount.

Start with adding the scripts to your `package.json`. The first will use the test runner to find all tests at the second level of your test folder. `--node-resolve` ensures that `import` statements find the files they need, and we're telling playwright to only run the webkit (Safari) browser. The second will do the same, only it will also rerun tests that have been modified.

{{< highlight json >}}
"scripts": {
	"test": "wtr test/**/*.test.js --node-resolve --playwright --browsers webkit",
	"testwatch": "wtr test/**/*.test.js --node-resolve --playwright --browsers webkit --watch"
}
{{< /highlight >}}


### TypeScript-Specific

If you're not transpiling from TypeScript, you can skip this section.

To integrate the esbuild function into our test runner we'll need to import its plugin into our test runner config. Call it `web-test-runner.config.js`.

{{< highlight js >}}
import { esbuildPlugin } from "@web/dev-server-esbuild";

export default {
	files: ["src/**/*.test.ts", "src/**/*.spec.ts"],
	plugins: [esbuildPlugin({ ts: true })],
};
{{< /highlight >}}

You may or may not need to create a `tsconfig.json` with the following settings, but I suspect you will. Oh for the day that twenty configs go away...

{{< highlight js >}}
{
	"compilerOptions": {
		"esModuleInterop": true
	},
	"exclude": ["node_modules", "dist"]
}
{{< /highlight >}}

{{< notice type=warning >}}
I had to add "type": "module" to my package.json file for tests to run. Probably related to esbuild, but ya never know.
{{< /notice >}}

## Test Our Component

With all that nonesense out of the way, let's write two simple tests. The first will make sure we're creating an instance of our web component, and the second will verify that we've wired up a property correctly.

Here's our web component. You'll have to do a little cleanup if you want to use it sans TypeScript, but it's not a lot.

{{< highlight js >}}
export class ChaosExample extends HTMLElement {
	get key(): string {
		return this.getAttribute("data-key");
	}

	render() {
		return `
			<span id="${this.key}"></span>
		`;
	}

	constructor() {
		super();
		this.innerHTML = this.render();
	}
}
{{< /highlight >}}

And here's our tests. I'll show you our helper function afterward.

{{< highlight js >}}
import { expect } from "@esm-bundle/chai";
import { fixture } from "../helpers";
import { ChaosExample } from "../../components/chaos-example";

describe("chaos-example", () => {
	it("builds a component", () => {
		customElements.define("chaos-example", ChaosExample);
		const el = fixture(`<chaos-example></chaos-example>`);
		expect(el).to.be.an.instanceOf(ChaosExample);
	});

	it("gets the data-key prop", () => {
		customElements.define("chaos-example", ChaosExample);
		const el = fixture(`<chaos-example data-key="test"></chaos-example>`);
		expect(el.key).to.equal("test");
	});
});
{{< /highlight >}}

You can abstract the custom element definition elsewhere, but I want it to be clear that we have to register our component with the browser from within our tests.

The magic that took me a bit of research and some help from [open-wc](https://github.com/open-wc/open-wc/blob/71ca5878900cdead78551d3c38b355d97dba9935/packages/testing-helpers/src/stringFixture.js) to solve is located in the `fixture` function.

There's nothing special about it, but there are a _lot_ of ways to insert elements into the browser DOM that won't work. This one does, and it also makes for an easy interface–just enter your HTML as a string and you're done.

{{< highlight js >}}
export function fixture(template, parent = document.createElement("div")) {
	const wrapper = document.createElement("div");
	wrapper.innerHTML = template;
	document.body.appendChild(wrapper);
	return document.body.lastChild.firstChild;
}
{{< /highlight >}}
