+++
author = "Alex Bilson"
date = "2023-04-14"
lastmod = "2023-04-26 11:18:42"
+++

The {{< backref src="/resume/call-report-parser" >}} is a project I've exclusively worked on for about a year. On more than one occasion I found that I had to dig through its internals to answer a question about how it worked or pulled up the [Swagger](https://learn.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-7.0) documentation page to run an adhoc query. When I mentioned that it'd be helpful to build a web interface for the parser my colleagues felt it wasn't worth the time. But how much time would it actually be? Turns out, just two days 🏎️💨.

I'm comfortable with Angular and or could hack the site with React, but why would I use heavy-weight tools for a light-weight project? Let's see how far the original tools have come, HTML, CSS, and JavaScript, since we all started down the framework highway.

## Design

I didn't need to worry about marketing design since this is an internal application. That saved me a ton of time and let me embrace the [CUBE CSS](https://cube.fyi/) methodology from scratch.

We've been running adhoc requests by editing a JSON payload in Swagger for far too long. Server-side validation kept us from making too many accidents with incorrect JSON parameters, but it takes mental effort to remember what choices I could pick. With this slick interface and the space to offer explanation, however, even a newbie could run an adhoc request.

{{< caption src="https://images.alexbilson.dev/call-report-generator-pg1.webp" >}}

The second issue with the Swagger approach is the need for supplimental information. Not only do I find myself explaining how the software works, I'm also asked to run some underlying queries that help us understand the result's origin. Exposing more endpoints to the Swagger page adds noise, but a couple of carefully designed tools on the web interface goes a long ways towards clearing things up.

{{< caption src="https://images.alexbilson.dev/call-report-generator-pg2.webp" >}}

Finally, another key source of information are rows in our database. While I'm very comfortable writing SQL queries, it's annoying to jump around to different places looking for summary information. Pull it into the web I say!

## Web Components

One of my biggest questions with this project was whether native [web components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components) could be equally productive to an Anguler app. Nearly every element on the page will need some interactivity, there will be several queries to an API, and I may want to communicate between components. Could all this be accomplished without a framework?

Below is the web component for the Get Bank Filers tool. It depends on one service, BankService, to retrieve data on form submission.

You'll notice I'm not even using a small framework like [Lit](https://lit.dev/). I've found Lit very intuitive and it would reduce some of the boilerplate. For example, the getters could use Lit's decorators and the events could be bound from the template itself. However, I didn't want to introduce a new build tool for my colleagues. This is ready for deployment without _any_ build process. Consequently, it was quite fast to iterate. Save and refresh.

{{< highlight js >}}
class ParserLatestBanksList extends HTMLElement {
    constructor() {
        super();
        this.bankService = new BankService();
    }

    get period() {
        return this.querySelector('select[name="period"]').value;
    }

    get since() {
        return this.querySelector('input[name="since"]').value;
    }

    get contentEl() {
        return this.querySelector('ol[name="content"]');
    }

    get errorEl() {
        return this.querySelector('p[name="error"]');
    }

    get messageEl() {
        return this.querySelector('p[name="message"]');
    }

    connectedCallback() {
        this.render();
        this.addEventListener("submit", (e) => this.onSubmit(e));
        this.querySelector('button[name="copy"]').addEventListener("click", (e) => this.onCopy(e));
        this.querySelector('button[name="clear"]').addEventListener("click", (e) => this.onClear(e));

        this.bankService.getAvailablePeriodOptions()
            .then((periods) => {
                if (periods !== null) {
                    this.querySelector('select[name="period"]').innerHTML = periods;
                } else {
                    this.errorEl.innerText = "failed to retrieve available periods for the dropdown";
                }
            });
    }

    disconnectedCallback() {
        this.removeEventListener("submit", (e) => this.onSubmit(e));
        this.querySelector('button[name="copy"]').removeEventListener("click", (e) => this.onCopy(e));
        this.querySelector('button[name="clear"]').removeEventListener("click", (e) => this.onClear(e));
    }

    onCopy(e) {
        const count = this.contentEl.children.length;
        const ids = Array.from(this.contentEl.children).map(x => x.innerText).join(',').replace(/,$/, '');
        navigator.clipboard.writeText(ids);
        this.messageEl.innerText = `copied ${count} ids`;
    }

    onClear(e) {
        this.contentEl.innerHTML = "";
        this.errorEl.innerText = "";
        this.messageEl.innerText = "";
    }

    onSubmit(e) {
        e.preventDefault();

        this.bankService.getLatestFedIds({ period: this.period, since: this.since })
            .then(
                (resp) => {
                if (resp !== null) {
                    this.contentEl.innerHTML = Array.from(resp?.fedIds ?? []).map(id => `<li>${id}</li>`).join('');
                } else {
                    this.errorEl.innerText = "failed to retrieve the latest bank fed ids";
                }
                },
            (err) => console.log("failed to retrieve the latest bank fed ids"));
    }

    render() {
        this.innerHTML = `
        <div class="call-report-form flow">
            <h3>Get Bank Filers</h3>
            <form class="flow">
                <div>
                    <label for="period">Period: </label>
                    <select name="period">
                        <option value="2023-03-31">2023 Q1</option>
                        <option value="2022-12-31">2022 Q4</option>
                    </select>
                </div>
                <div>
                    <label for="since">Since: </label>
                    <input name="since" type="date" />
                </div>
                <input type="submit" />
            </form>
            <hr />
            <ol name="content"></ol>
            <div class="spread">
                <button name="copy">Copy</button>
                <button name="clear">Clear</button>
            </div>
            <p name="error"></p>
            <p name="message"></p>
        </div>`;
    }
}
{{< /highlight >}}

You might notice is that there's no shadow DOM. That's right, I didn't include it in _any_ of my web components (though I would have if the need arose). That's because I want to control the styling from a single source and not need to replicate styles across my components for consistency. When a component does need specific styling, A single nested CSS block lets me put all of that design into one easy to find and edit place.

Another piece I'm proud of is the way that the HTML renders. Angular components I work with often show up only after all the data has been retrieved. If it's going to be noticably long we might have a loading display, but they tend to pop onto the page only after their data is ready. By splitting the initial render from the data hydration I minimize most of the pop-in effect. My period selection, for example, will load real, if incomplete, options and then update to the full set when the service returns.

I might have broken down and used a build tool at this point to use [SCSS](https://sass-lang.com/documentation/syntax) for nested selectors, but now they're part of the browser spec. Yes!

Because I'm using [CUBE CSS](https://cube.fyi/), a lot of the styling is handled at a higher level or with a small set of utilities. However, there were a few design choices specific to my tool forms for which a [block](https://cube.fyi/block.html) was ideal.

{{< highlight css >}}
.call-report-form {
    padding: var(--flow-spacing);
    border: 1px solid black;
    & h3 {
         text-align: center;
     }
    & p[name="error"] {
        color: red;
    }

    & ol[name="content"] {
        max-height: 200px;
        overflow-y: auto;
    }
}
{{< /highlight >}}

There's nothing especially magical about this CSS. But doesn't this feel nicely organized? Even if this component needed a ton more styling, native nesting and use of the regular DOM let's me scope all my styles to the right components without even a build tool. O Web, how far you've come.

One question that's impossible to tell from my screenshots is, have I made the web page responsive? Yes! With [Andy Bell's mantra](https://buildexcellentwebsit.es) "Be the browser's mentor, not its micromanager" ringing in my ears, I built the whole thing to flex with any screen size.

## State

No interactive website gets far before your faced with the need for communication between components. There are two common patterns: events and properties. There are many flavors of both–I won't delve into those details–but I did require a little of each. There may be a better way to handle either of these cases, but see how easy they are to implement!

### Cross-Component Notification

When there's a parent-child relationship, events can be bubbled up to the parent component. But when the components are siblings or far from each other in the DOM tree, you'll need either a top-level component to pass the event between them or add an event notification service. I went with the service.

There's not much to it. I put the event bus on the window object so I could globally refer to it and... we're done? Wow, that was easy.

{{< highlight js >}}
class EventBus {
    subscribers = {};

    subscribe(subscriber, callback) {
        this.subscribers[subscriber] = callback;
    }

    unsubscribe(subscriber) {
        delete this.subscribers[subscriber];
    }

    notify(message) {
        Object.entries(this.subscribers).map(([_, callback]) => callback(message));
    }
}

window.EventBus = new EventBus();

/* To emit a notification I add this to the component */
window.EventBus.notify("SESSION_CREATED");
{{< /highlight >}}

### Passing Properties

Parent-child component communication flows down with properties and up with events (usually). I didn't need any child-parent events for this project but it's all available in the [Event API](https://developer.mozilla.org/en-US/docs/Web/API/Event). But there was one instance that I needed to pass information from a parent to a child.

Now there are two ways to achieve this, properties and attributes, but I tend towards attributes for primative data. I don't need to pass along an object or array to my child component or this would be more complicated.

My Session History component has a button to get the jobs related to a session (jobs are simply records on another table). To retrieve the right set of jobs from the service I need to pass the session ID.

{{< highlight js >}}
/* this getter exists on the Job History component */
get sessionId() {
    return this.getAttribute("data-session-id");
}
/* and when the Session History component generates
   the Get Jobs button it adds the session ID as
   a data attribute. */
<td><button name="get-jobs" data-session-id="${i.id}">Get Jobs</td>
{{< /highlight >}}

That's a clean solution without decorators or template parsing. Nice 😎.

## Conclusion

I was a believer in native HTML, CSS and JavaScript before I started this project. Still, I'm shocked to build this web interface with no build tools in only two days. If it was going public there'd be more time required to get the brand design just right, but both functionality and layout are complete. I've never been more satisfied with the power and beauty of web development than I am today.

## Riposté

Thanks to a [comment](https://mastodon.social/@westbrook/110205893240016971) from [Westbrook Johnson](https://indieweb.social/@westbrook@mastodon.social) I broke my web components and services into separate files using [Import Maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap). Now I have all the organization of a transpiled project without any transpiling step!
