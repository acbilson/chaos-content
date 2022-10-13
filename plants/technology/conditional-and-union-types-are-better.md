+++
author = "Alex Bilson"
date = "2022-10-13 09:54:17"
lastmod = "2022-10-13 10:04:14"
epistemic = "sprout"
tags = ["snippet","typescript","errors"]
+++
Elsewhere I advocate for that developers {{< backref src="/plants/technology/use-http-response-objects" >}}. TypeScript has a couple nifty improvements that can really make these response objects simple: Union and Conditional types.

Let's say you have an Author object returned from your API. Now you want to add response properties. [Type Union](https://www.typescriptlang.org/play?q=416#example/union-and-intersection-types) to the rescue!

{{< highlight js >}}
export interface Response {
  success: boolean;
  message?: string;
}

export interface Author {
  id: number;
  name: string;
  age: number;
}

// the union is an ampersand
type AuthorResponse = Author & Response;

// and a typed example
const x = <AuthorResponse>{
  success: true,
  name: 'Alex',
  age: 34
};
{{< /highlight >}}

That worked great for your /artist GET request, but now you need an /artist PUT. But you just need to edit the age and leave the name null. And now for [Mapped Types](https://www.typescriptlang.org/play?q=99#example/mapped-types).

{{< highlight js >}}
// this works more like a type builder func than a type
type MyPartialTypeForEdit<Type> = {
  [Property in keyof Type]?: Type[Property];
} & { id: number };

type AuthorEdit = MyPartialTypeForEdit<Author>;

// and a typed example
const y = <AuthorEdit>{
  id: 1,
  age: 35,
}
{{< /highlight >}}
