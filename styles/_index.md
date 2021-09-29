+++
Title = "Styles"
displayInMenu = false
kind = "page"
+++
This page is to demonstrate and test styles, from the basic styles to my custom shortcodes. Since I don't use these everywhere, I can go here to figure out how they'll look with style changes and test interactions.

{{< raw >}}

<h1>Text</h1>
<section class="style-bordered">
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
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
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
  I am a monotonic robot
  </code></pre>

</section>

<h1>Colors</h1>
<section>

  <div class="style-circle-container">
    <div class="style-circle style-circle-bg"></div>
    <div class="style-circle style-circle-bg-offset"></div>
    <div class="style-circle style-circle-txt"></div>
    <div class="style-circle style-circle-txt-offset"></div>
    <div class="style-circle style-circle-border"></div>
    <div class="style-circle style-circle-primary"></div>
    <div class="style-circle style-circle-primary-offset"></div>
    <div class="style-circle style-circle-secondary"></div>
  </div>

</section>

{{< /raw >}}

# Shortcodes

## Text

This is some text that contains an {{< outref src="http://duckduckgo.com" name="outref" >}}. After it comes a backref for a note: {{< backref src="/notes/top-of-mind" >}}.

Here's an acronym: {{< acronym "MBA" "Masters in Business Administration" >}}. And if I wanted to cite myself, here's how{{< cite name=Bilson loc="pg. 97" >}}.  But maybe I just wanted to make a note{{< superset 1 "addendum" >}} of something.

{{< highlight sh >}}
ls | rg -i 'stuff'
{{< /highlight >}}

## Sections

{{< raw >}}<section class="style-bordered">{{< /raw >}}

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
src="https://bn02pap001files.storage.live.com/y4mYunje7Ps5pTlKWXvzNovYS0y4Oemfv5RDbyug0hRyK-ILG3pw9pDiZg6w4wwtFzj3jKDT8ex4GF0e9-rkbAUADIyeBeJFDYkXTHfByIl0dn6xZt3T8EqtIsbJycCuYZgDYb5pqGPGI9x5QUttmPYuztKAN-VHBjmP_uy25W_acp3NVgutzihWT6m0jFQf5c_?width=768&height=1024&cropmode=none"
alt="Royal Plays in Diamond"
caption="This is an optional caption for this image. Isn't Royal cute?"
>}}

### Poetry

{{< poetry >}}
  These are the generations
  of the heavens and the earth when they were created,
  in the day that the LORD God made the earth and the heavens.
{{< /poetry >}}


{{< raw >}}</section>{{< /raw >}}

## Special
{{< raw >}}<section class="style-bordered">{{< /raw >}}

{{< table
title="Programming Languages"
headers="Language|Acumen"
rows="C#|9^Python|8^JavaScript|8^T-SQL|7^Rust|4"
examples="Budget App|https://github.com/acbilson/plaintext-budget^chaos-micropub|https://github.com/acbilson/chaos-micropub|https://github.com/acbilson/statement_converter^chaos-hugo-theme|https://github.com/acbilson/chaos-theme"
>}}

{{< vocab
  word="Beesh"
  term="noun"
  phonetic="bēsh"
  match="Beach"
  example="Go beesh?"
>}}


{{< raw >}}</section>{{< /raw >}}
