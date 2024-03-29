+++
Title = "Styles"
displayInMenu = false
kind = "page"
+++
This page is to demonstrate and test styles, from the basic styles to my custom shortcodes. Since I don't use these everywhere, I can go here to figure out how they'll look with style changes and test interactions.

{{< raw >}}
<section class="flow-l">
<article class="flow-m">
  <h1>Colors</h1>
  <ul class="spread style-circles" role="list">
    <li title="primary"></li>
    <li title="secondary"></li>
    <li title="tertiary"></li>
    <li title="stroke"></li>
  </ul>
</article>

<article class="flow-m">
  <h1>Heading Text</h1>

  <h1>Big Title</h1>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  <hr />

  <h2>Medium Title</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  <hr />

  <h3>Small Title</h3>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  <p>Lorem ipsum dolor sit amet.</p>

  <blockquote>
  <p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
  </p>
  </blockquote>

  <ul>
    <li>This is a list</li>
    <li>An unordered list</li>
    <li>Do you like?</li>
  </ul>

  <ol>
    <li>This is a list</li>
    <li>An ordered list</li>
    <li>Do you like?</li>
  </ol>

  <pre><code>
  I am a monotonic robot, which means something about me only having one track.....
  </code></pre>

</article>


<article class="flow-m">
{{< /raw >}}

# Shortcodes

## Text

This is a bit of text with a {{< raw >}}<span>span</span>{{< /raw>}} in it.

This is some text that contains an {{< outref src="http://duckduckgo.com" name="outref" >}}. After it comes a backref for a note: {{< backref src="/gardens/business/business-book-list" >}}.

Here's an acronym: {{< acronym "MBA" "Masters in Business Administration" >}}. And if I wanted to cite myself, here's how {{< cite name=Bilson loc="pg. 97" >}}.  But maybe I just wanted to make a note{{< superset 1 "addendum" >}} of something.

{{< highlight sh >}}
ls | rg -i 'stuff'
{{< /highlight >}}

{{< notice >}}
This is a bit of information.
{{< /notice >}}

{{< notice type=warning >}}
<b>WARNING:</b> This is an alert.
{{< /notice >}}

{{< notice type=quote name=Anonymous short=true >}}
This is an anonymous quote
{{< /notice >}}

{{< raw >}}
</article>
<article class="flow-m">
{{< /raw >}}

## Sections

{{< accordion name=Accordion >}}
This is the content of my accordion example.
{{< /accordion >}}

{{< addendum >}}
This is the first note in my addendum. It's going to be the longest note so I can see how it looks when it overflows.
And the second.
And the third.
{{< /addendum >}}

{{< citation >}}
Bilson, Alex. Chaos and Order (2021).
Bilson, Alex. Chaos and Order, Mark II (2021).
{{< /citation >}}

### Image with a caption

{{< caption
src="https://images.alexbilson.dev/20210626_185256743_iOS.webp"
alt="Royal Plays in Diamond"
caption="This is an optional caption for this image. Isn't Royal cute?"
>}}

### Poetry

{{< poetry title="Genesis 2:4">}}
  These are the generations
  of the heavens and the earth when they were created,
  in the day that the LORD God made the earth and the heavens.
{{< /poetry >}}


{{< raw >}}
</article>
<article class="flow-m">
{{< /raw >}}

## Special

{{< vocab
  word="Beesh"
  term="noun"
  phonetic="bēsh"
  match="Beach"
  example="Go beesh?"
>}}

{{< raw >}}</article></section>{{< /raw >}}
