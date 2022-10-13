+++
author = "Alex Bilson"
date = "2021-02-10T15:54:31"
lastmod = "2022-10-13 10:05:44"
epistemic = "sprout"
tags = ["snippet","python","errors","design"]
title = "Use HTTP Response Objects"
+++
As Victoria mentions in her [post](https://victoria.dev/blog/do-i-raise-or-return-errors-in-python/), the type of code you're writing affects how you'll handle errors. Her example matches code that integrates with other systems on the same machine (and probably other cases).

The code I'm writing these days, when it's not JavaScript, is for back-end services. I've discovered that services delivered over HTTP/S are best implemented to capture exceptions in the service and return response objects. I'll usually do something like this:

{{< highlight js >}}
class Response<T> {
	constructor(
		public success: boolean,
		public message: string,
		public result: T,
	) {}

	public static default<T>(): Response<T> {
		return new Response(false, null, <T>null);
	}
}

interface ReadResult {
	public name: string,
	public age: number
}

// and to use it in a function
function readStuff() {
	this.api.getStuff()
		.then(r => r.status === 200 ? <Response<ReadResult>>r.json() : null)
		.then(r => {
			if (r.success) {
				console.log({ name: r.name, age: r.age });
			} else {
				console.log(`failed with message: ${r.message}`);
			}
		})
		.catch(err => console.log(err));
}
{{< /highlight >}}

Consumption of this kind of API is straightforward and reduces the amount of error handling necessary in the front-end code. I don't know that I'd recommend this for writing a library to read data from the file share, but for a web API, absolutely. Thoughts?

If you're using TypeScript like my example above, read on to find that {{< backref src="/conditional-and-union-types-are-better" >}}.
