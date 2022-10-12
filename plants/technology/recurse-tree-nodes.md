+++
author = "Alex Bilson"
date = "2022-10-12 14:30:31"
lastmod = "2022-10-12 14:39:25"
epistemic = "plant"
tags = ["snippet","javascript"]
+++
Reasons to build recursive functions happen infrequently enough that I often forget how to do it. Here's an example I wrote recently to retrieve a flat array of nodes in a tree which meet a certain requirement. Essentially it's a recursive filter. It's been made generic so that any object which implements the Node interface can use it.

{{< highlight js >}}
export interface Node {
	public parent?: Node;
	public children?: Node[];
}

export function getNodesBy<T>(propFunc: (Node) => boolean, tree: Node[]): T[] {
	const getNodes = (items: Node[]) => {
	const matches = items.filter(i => propFunc(i));
	return items.some(i => i.children?.length > 0) ? matches.concat(getNodes(items.flatMap(i => i.children))) : matches;
	};
	return <T[]>getNodes(tree);
}
{{< /highlight >}}
