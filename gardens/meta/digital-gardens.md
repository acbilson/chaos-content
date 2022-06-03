+++
author = "Alex Bilson"
date = "2021-11-08"
lastmod = "2022-06-03 14:28:32"
toc = true
narrow = true
title = "Design Delta"
[coordinates]
    x = 200
    y = 960
    step = 60
    vertical = false
    bend = -20
+++
This is a meta garden; a garden full of ideas, dreams, and sketches for my "digital garden" - the site you're on right now.

There's not an official definition for the term, "digital garden," but the best overview of the concept is Maggie Appleton's [History of the Digital Garden](https://maggieappleton.com/garden-history). You might also glean insight into the differences between a digital garden and a blog by reviewing Shawn's [Digital Garden Terms of Service](https://www.swyx.io/digital-garden-tos/), or by Tom Critchlow's analysis of three forms of digital communication in [Streams, Campfires and Gardens](https://tomcritchlow.com/2018/10/10/of-gardens-and-wikis/).

## Content Concepts

Implementations of the digital garden concept vary widely, but among the most cohesive I've found two that serve as useful representations.

- {{< backref src="/plants/meta/gardens-as-museums-of-others-content" >}}
- {{< backref src="/plants/meta/gardens-as-networks-of-private-insight" >}}

## Content Growth

Digital gardens use the imagery of a garden to explore the maturity level of the garden's content. They stand in contrast to a database of completed work (traditional blogs) or a stream of thoughts (social media). As a plant can begin a seed, grow into a sprout, and mature to a flower that itself bears seeds, so can an idea or concept that's encapsulated in one's content. This analogy assists content creators to differentiate their work among various classifications, such as its polish, their certainty of its reliability, or the breadth of humanity for which it may be applicable. An exploration into the digital garden must, by this analogy, also delve into the ways that plants arrive in the garden and how they are classified.

There are two core classifications, fluid and stable. A digital gardener must, even if it's never classified on their site, {{< backref src="/plants/meta/define-fluid-and-stable-content" >}}.

## A New Analogy

Riffing off Tom's streams metaphor, let's borrow the analogy of a boats sailing in a bay to explore the formation of a digital garden.

> It's probable that the garden analogy could be expanded to fit, but I was a sailor so there ya have it. When I think of a garden, I think of a small plot of land with a few flowers and vegetables. If you use the Queen of England's gardens for an analogy, now you've got a lot more room to _grow_.

A digital garden is like a boat-filled bay fed by many streams. Each stream has it's inlet where it joins the bay. The bay has limited physical barriers to the ocean beyond, so most boats that float down the streams join with the ocean. Some boats, however, stay within the bay.

The streams that feed into the bay are data streams. A Twitter timeline might feed into the garden, or an RSS feed. For the sake of simplicity, even reading a book or watching a movie would qualify since digital gardens aren't aggregations of data, their external representations of an individual's thought (#MORE_HERE). Let's not lose sight that a garden is curated by humans, not mass-produced by bots, while we try out this alternative analogy.

The boats floating in the stream are cohesive units of content. Every person has their own threshold for which boats put up anchor in the harbor, but generally only a fraction of all boats are worthy of inclusion, like a few quotes from a book vis-à-vis the entire volume.

Inclusion does not equate to value. I may write journal entries to share the latest family updates with my family and friends. Just because it's mostly excluded doesn't make it drivel. And some of those updates are raw material for family stories that may be filled out and planted in my garden.

## A New Site Vision

I'm tossing around a few tweaks to my own site after exploring the digital garden / digital bay concepts.

![Site Design Wire Frames](https://bn02pap001files.storage.live.com/y4mYl9__E7DIJPiaRvoOfotn1CLFm3kZ3XJ1CIRPuJsdG4puB1r5naOTVqmZKafR6YxcrtMgkSpWhnxxvfQRIicpYpxLdgZgOiKmyBIpGEhOdY3m5wgLAbjCPCHnn_73HgzXuAMI2jVxJwzkcZyI57ts-_rMz040M7Z1g5ZXImHVVtPgsLcPAYkXTMoCTaAhXsq?width=768&height=1024&cropmode=none)

For my site, I _do_ want everything to flow into my own garden, no other tools required. Since my fluid content and my stable content are within the same system a clear definition of the two types can be explicitly filtered into boats (fluid) and rocks/plants (stable).

### Log

A log is a fluid, chronological journal entry about me or my family. This content is displayed in the river as a boat. It's not included in other views, but can be clicked to open an individual log entry.

### Quip

A quip is a fluid, non-chronological entry that collects some idea or piece of content. It might be a flash of inspiration, a bookmark, a quote, etc. The content itself is all that's important; it does not require anything more than a timestamp. It will show up in the river as a boat of a different color/style.

### Plant

A plant is a stable entry of personal content. It might contain references to other's content, but it's core purpose is to encapsulate a gardener's personal thought. It is a Quip that's been expanded to include context and structure, such as a title or references. It's an organic entity that is open to further growth or pruning, but it has moved from the temporariness of the fluid boat to a place on the land.

### Stone

A stone is a stable entry of other's content. It encorporates full attribution to the original creator but, since it's not under the creator's control, is unlikely to change.

## Views

### Bay View

I'd love to have an iconic bay view that shows everyting from a 10,000 foot perspective. All the tributaries can be seen, like small ribbons leading into the bay. Small islands will be shown. The whole picture will have an ancient map feel. Entries that have recently been added to a tributary would be shown as a little boat floating towards the bay islands. Each of the tributaries or islands will be clickable.

When clicked, the view will zoom to that tributary/island. The view may be slightly different depending on its content. For example, the log tributary (based on ship's log) will show the Story Island at the top and entries floating towards it in little boats.

If I click an island, it will zoom to a second-level view where the island takes up the screen. Island features will be labelled and clickable. For example, on Story Island, "Poetry Point" would be written along a prominent outlook. On Treasure Island, a "Quote Quay" might be available to click.

These views are two-fold. First, they give the viewer a sense of continuity amonst all the content while illustrating the differences between each. Sailboats are moving towards a destination, not static, while content on "Poetry Point" might be subject to revision but not removal. Second, they help organize the flow of information such that I will more easily be able to organize the entry and dissemination of information.

Everything in this view will be a type. Broadly, there are two types: standing rocks (on the island) and fluid boats (in the tributaries). But the content in "Quote Quay" will be only quotes.

This is the high-level overview display, but it is siloed in a way. Poetry will belong to "Poetry Point", but a poem about work that might be referenced by an article on Treasure Island has no way to display that link. And so the next view is born.

### Treasures and Plants View

This view is about connection, therefore content that's in the tributaries drops off. Connection can be produced in two ways: by reference and by tag. These will be interchangable in some way.

Treasure is gathered from an external, static, source. A quote from a favorite book is an example of a treasure. These are not subject to change and contribute, but do not form, my own content. They are emminently referencable material, but they don't 'grow'.

Plants, by contrast, are growing, changing, personal content. An insight about the nature of humans is an example of a plant. The insight may be supported by treasures, such as a quote from the Bible or a digital article, but it is an insight which I have written about and may edit, delete, or refute.

### Tag View

The tag view will match something like Nick's commonbook. What's crucial is that the tag/author (or space/creator) is easy to reference and peruse. A count per would be helpful, just like his site. Unlike his site; however, there will be at least two kinds of content: treasure and plant.

I will use the tag view to explore concepts. For example, like Nick's site I may want to review everything I've learned/gathered about work. A link to 'work 5|0' indicates there are five treasures and zero plants in this category (a good candidate for personal reflection). If I click the link, I will be shown a screen/panel/something which might include a quote, a piece of poetry, a story, etc. These would belong on different islands, but here they are together. I don't know if I also want creators like Nick has - I am already separating mine/not mine, which may be enough?

### Reference View

The reference view will be a network diagram. It will show, at telescopic levels (like Obsidian), what content links to and is linked by. A plant, a MOC, may be called "Design Patterns" and have three plant notes and two treasure nodes that it refers to. The plant nodes will have backrefs to the MOC, but of course the treasures are external and will not. On this view also these two types, plant and treasure, must be visibly different.

Both plants and treasures may have node names that are descriptors, e.g. "Design Patterns", or action statements, e.g. "Choose Leaders Wisely." Therefore they have to show some designation which makes them appear different, whether by color or icon or something else.

I will use this view to explore connections and missing insights. For example, I may find a hub node which shares a single child node with another hub. This node connection is worth exploring to see if there are other similarities between the two hubs.

## Building Blocks

This is an enormous undertaking. How can I break it down?

For a v1, I have some pieces already. chaos-micropub gives me the ability to create logs anywhere (awesome). Notes are a bit more limited since I _must_ enter a name, so perhaps I need another boat-style fluid entry that I could use which doesn't produce something in the log stream OR the notes (like a draft?).

Rocks exist in the form of posts and notes for plants, but a treasure type must be added. Posts/notes would need to be re-envisioned to form a single new type, a plant, but how to chop it up is not known at this time. For example, is it best to have a system that separates plants into types, e.g. plant/story? What other ways, like epistemic, lastmod, etc, can be used to filter and display them? Should plants and treasures be in separate directories, like /plant and /treasure, or should it be by type, e.g. poetry/plant, poetry/treasure, or even just /poetry with differing metadata? It's at this point I'd rather move to sqlite...

Another question set is, should one system contain both boats and rocks? Should the input stuff flow in one system, and rocks be stored in another? Like boats being chaos-micropub and rocks being chaos-wiki? Oh the questions...

One way around the folder structure might be to use stubs. I could incorporate any level of foldering without having to worry about it affecting the high-level routing. For example, I could have plants and statues in separate folders for writing/editing organization, but have them hosted at the same route. Same with logs and unnamed notes.

/log
/quip
/plant
/stone

Instead of posts and notes, there will be only plants and stones. The post page will become a mature plant, so that any note which reaches maturity is discoverable.

### Type: Log

The log type will suffer the least change. It will remain in timeline format, but with an interesting boat-style theme floating towards Bilson Bay. These may be promoted to stories at some point, which would put them in the plant type.

### Type: Quip

This type doesn't exist today. Like the Log type, it will have no title. It is by nature ephemeral, meant only to house thoughts and ideas in-flight for later addition as plants or stones. It's possible that this type might be skipped if the creation of plants and stones were simple (worth considering). Alternatively, it could be a plant/stone with draft = True so it doesn't show up.

### Type: Plant

This type matches the note type today. However, it also comprises the post type. The difference between the two will be only its growth and epistemic level. A newly-formed plant is likely to have a low epistemic level, perhaps 'hypothesis', and an early growth value, probably 'seedling'. A plant which belongs on the Home page, the Island if you will, has at least a growth value of 'plant'. Epistemic level may also play a factor. In this way plants are separated into different places on my website. See epistemic ideas [here](http://evantravers.com/articles/2021/10/25/nurturing-notes-and-thoughts/)

### Type: Stone

This type has no correlative today. It comprises any content, whether it's a quote, a story, a design, etc. which I do not possess. It cannot grow because it's a snapshot of another's work. I'm not sure yet whether this type requires an epistemic value, but it should have a longevity value, I'm thinking "fresh", "worn", "mossy". Perhaps this _does_ incorporate the epistemic value, for it assumes that the time-worn holds more weight. Alternatively, there value could be calculated from the creation date: fresh being 0-3 months from creation, worn being 4-12 months, mossy being over a year. The purpose of having this value, like the growth value for plants, is to divide stones into two distinct places: on the island and off. Those stones which make it "on the island" are crucial to the formation of my ideas and life, while those not yet on the island are being tried out for veracity and usefulness.

## Resources

### For making a map-style interface

- [Pencil Effect](https://webdesigntips.blog/web-design/css-tricks/creating-a-pencil-effect-in-svg/)
- [Effects and Inspiration](https://heredragonsabound.blogspot.com/2021/10/creating-pencil-effect-in-svg-part-2.html)
- [Map Inspiration](http://mewo2.com/bots/mythicmaps/)
- [Inspiration](https://www.lordofthemaps.com/)
- [Animate Along Path](https://codepen.io/yshlin/pen/dxGlH/)
- [Inkscape Simplify](https://inkscape.org/doc/tutorials/advanced/tutorial-advanced.html)
- [Inkscape Caligraphy](https://inkscape.org/doc/tutorials/calligraphy/tutorial-calligraphy.html)
- [Map Collection For Tracing](https://www.davidrumsey.com/)

### For exploring other tools

[TiddlyWiki](https://tiddlywiki.com/)

- the only limitation to this one is figuring out how to host the tiddlers separate from the core empty.html file. I'd rather use Git to version control my content.
- this is resolved, but I'm not convinced this is the best route for me to go.

## Notes to Self

DON'T MIX METAPHORS!!! I'm tempted to create special pages for different gardens; maybe a tower here, a town there, but this leads to confusion. Alternative garden displays, configured in chaos-themes, is okay. Having a special layout for a specific garden is not okay. If I'm going to do that, I may as well make it a separate page entirely.

Some of my content is more training/tutorial/how-to oriented than evergreen-note-oriented. These should be extrapolated to one or more gardens, such as a tech how-to garden, or canibalized into plants.

Should the tag and author directories be displayed differently on the island? They're not gardens per-se, but more like reference books. Libraries? They still represent crucial stopping-places on the map, for they're the only place to review **all** plants/stones.

Put a garden definition at the top. Something like "A garden is a free-form space that's composed of plants and stones, with its own unique structure and purpose. A garden blurs the lines between MOCs, pages, and new sites"

My focus has been chiefly on the web presentation, but there's also a need for better author tooling. For example, it'd be nice if I could generate a new plant or stone in Vim with a simple keystroke. Or to create a simple script, run with `entr | script`, that might list to the console all other files with the same tags as the last edited file, and maybe backlinks too. I've done some work with chaos-micropub already, and this might be a better end game option. I'll have to think about it.

A MOC loosely translates to a garden, but I reserve the power to make beautiful, organic changes on a per-garden basis. It's not a neat link-aggregate. Some gardens may have many links, others may be more self-contained. **There is no desired garden end-state.**

Gardens, stones, and plants marked 'evergreen' should have fairly stable URLs. Everything else may need a simple warning that it's location is subject to change.

You "visit" a garden (sit, enjoy, rest)
You "view" a plant/stone (browse, inspect)

It may make sense at some future point to separate my site into alexbilson.dev and ofchaosandorder.com. My about page, resume, etc, the content that's specific to me and is for outsiders to get to know me, would be on alexbilson.dev. The garden would be on ofchaosandorder.com. (.com? .garden?)

Hugo makes gardens of text and links easy, but styles of gardens, esp. with generated content lists and such, require separate content types. I could use front-matter like type = "english-walled" to build specific garden flavors, but the build automation, at least the automation that needs the build context, is not available to the page itself. The front-matter is all that's available from the page. A separate data template can only add complexity to the values available at build, but for entirely different HTML structures it takes a new content type.

## Illustrations

Now that I've made a foray into web illustration, Maggie Appleton's list of [Illustration Books & Courses](https://maggieappleton.com/illustration-resources) is my new favorite resource! I may not ever achieve Maggie's expertise, but I'd like to draw simple illustrations and diagrams to explain concepts and to pretty up my garden.

## Towards a Standard Flow

I've encountered a few obstacles now that this vision is _mostly_ realized.

1. There's **a lot** of content. I recently exceeded 400 plants, 250 logs, and 80 stones. I've implemented some navigation options, such as full-text search, a tags/authors page, and plant filters, it remains an overwhelming amount of stuff to review.

2. I've lost track of _why_ I'm doing this. Endless addition or revision of my site is not meaningful in and of itself. I maintain a log for family and posterity which has gotten a lot of positive feedback but the rest of the site is... complicated.

Thanks to Baldur's research in {{< backref src="/plants/writing/note-taking-tools-and-strategies" >}}, I've realized there are a few concepts that were fuzzy to me when I began creating a "digital garden."

### Stuck on Contextualization

I'm stuck on stage #2 of the {{< backref src="/plants/writing/note-taking-process-towards-an-end" name="Note Taking Process" >}}, Contextualize. I've developed Collect habits and I have a willingness to synthesize what I've written into the larger picture, but there's no Map stage. Without a central project to accomplish that gravitates notes into it's orbit, motivation to draw note connections and draft summaries dries up. Much of my recent note-taking has circled around the subject of system administration and automated deployment precisely because I've been working on a project to deploy my site and it services to the Cloud in preparation for our move.

Once I begin to think in terms of writing projects, my motivation juices begin flowing again. I have over 200 business insights but no motivation to do more with them. However, the thought of crafting them into a consulting guidebook intrigues me greatly and might be sufficient reason to review/edit/compile them into something new.

But creating more projects for myself isn't the direction I'd prefer. I've got lists of private project ideas. What intrigues me about this writing roadblock is how I might use this discovery towards formation. I may not have writing projects that I really want to complete, but I do have ways of being in which I'd like to be formed. For example, I want to be:

- a wise and gracious father and husband
- a trusting and faithful disciple of Jesus
- an organized and proficient software engineer
- a welcoming and consistent host

Formation is habit-based over project-based, although I suspect that projects might be tailored to achieve certain formative goals. An arduous work project certainly offers the chance to develop humility and patience.

Habits tend to be helped by journal-like entries and reflection. These fit the stream concept more than the digital garden. But projects could still be selected under these formative goals, like explaining our parenting strategy for conflict resolution. It's fuzzy right now, but the effort to write a guide would spur research and clarify the subject.

### Longing for Social Interaction

The discovery that I'm stuck in the Contextualization stage surfaces another desire - I want more idea interaction with others. This desire is why I created the reminder plant, {{< backref src="/plants/writing/reply-to-others" >}}. Individual posts rarely feel worthy of an email to someone to "look at this and tell me what you think." But if I were working on a thesis, I'd have reason to pull in other voices. "How do humans become more patient?” is a fruitful dialog, “what do you think of my post about types of patience” is not (though such a post may be written in pursuit of the original thesis).

The first barrier to crafting an in-network thesis is fear that it's already been explored. I don't want to redo the work of a recent writer. From the standpoint of adding to human knowledge, redo is waste, but from the standpoint of formation it's more effective to independently research a thesis than to read the summary of another's research. If knowledge is primary, people will assist to be part of a new branch of research. But if formation is primary, people will assist only if they too want to be formed by the thesis subject.

### Solution: Thesis Design

To motivate by offering concrete projects that interest me and to enable the contribution of outside voices, I propose a thesis design approach.

Theses questions are compiled in gardens. Thesis statements are listed in answer to the question, and these are plants. Because the question and the answer are tied, they ought probably to be in the same file?

Theses need an abstract that will display when a link is highlighted to build upon earlier theses. For example, a thesis about spiritual disciplines in DBS may have support from theses on the value of spiritual disciplines and the centrality of DBS. I want to highlight the link and read the abstract to see how it supports my current hypothesis.

Perhaps this is the reasoning behind epistemics like "certain, hypothesis, wild guess" as differentiated from growth? I wonder if that would be more useful than growth or if both should co-exist?

I also wonder, is this how my site becomes multiple? A single domain, say parenting, might benefit from having a sequestered list of theses. Not gonna do that right now though.

Let's generate a couple gardens as an experiment, {{< backref src="/gardens/faith/disciple-making-theses" >}} and {{< backref src="/gardens/parenting/parenting-theses" >}}.
