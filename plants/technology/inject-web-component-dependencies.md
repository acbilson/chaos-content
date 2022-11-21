+++
author = "Alex Bilson"
date = "2022-11-21 14:14:02"
lastmod = "2022-11-21 15:13:49"
epistemic = "plant"
tags = ["snippet","javascript","dependency","web-component"]
+++
If you're transitioning from a framework like Angular to native web components it feel natural to inject services into your components. When you discover it's not possible via the constructor, you may give up and use a framework after all. Or you might [engineer your own dependency injection decorators](https://www.thinktecture.com/en/web-components/dependency-injection/). But there's another lightweight option. Let me show you.

{{< notice >}}
These instructions are perfect for small, private use-cases. If you're building dozens of services with interlocking dependencies, there's no decent option besides a framework. Sorry.
{{< /notice >}}

## Lightweight DI

For my use-case, the need for dependency injection came along at the same time that I wanted to {{< backref src="/test-web-components-with-playwright" >}}. My most complex logic is nestled inside API fetches.

I tried to pass my services via the constructor first, but web components are built by the browser without parameters. I could programmatically generate my components, but that sucks. I want to put them into HTML just like any other tag, not write a special bootstrapper.

Thanks to Yannick's [solution](https://www.thinktecture.com/en/web-components/dependency-injection/) I discovered that you could use CustomEvents to reach outside a web component for it's dependencies in the `connectedCallback` function. But I didn't want to replicate the root component approach; it feels too much like building my own framework. Instead of a root component, I use the document root. Let's look at some code.

## Explanation

The first place we'll visit is my simplified injector on the document root.

{{< highlight js >}}
document.addEventListener("chaos-request", (e: CustomEvent) => {
	const request = <InjectionRequest>e.detail;
	// InjectorMap is a TypeScript Map<string, object>
	const instance = InjectorMap.get(request.instance);
	request.callback(instance);
});

// custom element definitions go below
{{< /highlight >}}

My build output sources from an `index.ts` file which defines each custom element I'm using for the project. I add my `chaos-request` event listener just above the component definitions to ensure that it's ready when the component `connectedCallback` functions start to request their dependencies.

While I _could_ have InjectorMap instantiate my services when they're called, they're all singletons. No reason I shouldn't instantiate them at Map creation then.

{{< highlight js >}}
document.addEventListener("chaos-request", (e: CustomEvent) => {
	const request = <InjectionRequest>e.detail;
	// InjectorMap is a TypeScript Map<string, object>
	const instance = InjectorMap.get(request.instance);
	request.callback(instance);
});

// custom element definitions go below
{{< /highlight >}}

The `InjectionRequest` object is equally simple. Just a an instance name for the type I want, and a callback through which I can send it back to the requesting component. I might have used the object's type, but I chose a string value for better vanilla JavaScript interoperability.

{{< notice type="warning" >}}
I didn't want to deal with interlocking service dependencies, but I did need to share state management. I chose to allow a global store that my services could share and pass along an instance via my dependency injection. This did mean that services which depend upon my store don't have complete internal DI. That's a problem for another day.
{{< /notice  >}}

Can you believe that's all it took to build a dependency injector? No bells-and-whistles, but now we can abstract our web component services! Let's look at a web component now.

{{< highlight js >}}
connectedCallback() {
	const getPublisherEvent = new CustomEvent(
		"chaos-request", {
			bubbles: true,
			composed: true,
			detail: <InjectionRequest>{
				instance: Instances.PUBLISH,
				callback: (e) => (this._pub = e),
			}
		});
	this.dispatchEvent(getPublisherEvent);

	// can start using this._pub now
}
{{< /highlight >}}

The web component emits a CustomEvent request that bubbles up to the document root for fulfillment. Since this event runs synchronously, we can immediately begin using our private service anytime after the event dispatch.

There's more you could add to make abstract away the details, but that's a pretty great start!

## Writing a Unit Test

We went through all that trouble so we could write unit tests, so let's look at one.

{{< highlight js >}}
describe("chaos-panel-option", () => {
	const authorized$ = new Observed();

	before(() => {
		const injectorMap = {
			"store-state": {
				isAuthorized$: authorized$,
			},
		};

		document.addEventListener("chaos-request", (e) => {
			const request = e.detail;
			const instance = injectorMap[request.instance];
			request.callback(instance);
		});

		customElements.define("chaos-panel-option", ChaosPanelOption);
	});

	it("renders unauthorized text if not authorized", () => {
		const el = fixture(`<chaos-panel-option></chaos-panel-option>`);
		authorized$.value = false;
		expect(el.innerHTML).to.contain("Not authorized");
	});
});
{{< /highlight >}}

I won't go into the test setup here. You can find that in this article: {{< backref src="/test-web-components-with-playwright" >}}.

I've added a custom Observer implementation to my codebase you could think of kinda like Angular's `Observable` type. The `fixture` helper appends my web component to the document DOM.

You'll notice that the injector pattern is identical to what we've already reviewed. Its only difference is that it uses a mocked Map object so that I can pass along my own stubs.

The test works like this.

1. At the start of all test's execution, the injector event listener is added to the document root. I retain a reference to the part I'll change inside my tests.
2. The test adds my web component to the document DOM. Then I use my reference to emit a store value, setting authorized to false. My component subscribes to this store value.
3. I check my component's rendered HTML to ensure it shows the right message, "Not authorized".

{{< notice type=warning >}}
It is necessary to add the web compoonent to the DOM prior to updating the store value. Otherwise its event listener will not be prepared to "hear" the store update event.
{{< /notice  >}}
