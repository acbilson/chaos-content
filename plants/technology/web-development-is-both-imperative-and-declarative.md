+++
author = "Alex Bilson"
date = "2023-03-06 13:11:49"
lastmod = "2023-03-06 14:06:34"
epistemic = "plant"
tags = ["javascript","html","css","design","programming","paradigm"]
+++
Jeremy Keith explores Imperative/Declarative web development and how team culture defines the paradigm a project chooses more than the language itself in [this Youtube Video](https://youtu.be/UHfa5ks4q80). It's insightful and short: only 22 minutes.

Jeremy defines imperative programming in terms of control.

- Imperative programs stipulate each step (instantiate this array, iterate over this collection, put items that meet this criteria into the instantiated array). Side-effects are minimized, but the code must also describe everything that must be accomplished.  Maximum control, maximum verbosity.

- Declarative programs describe the final result and leave much of the calculation to arrive at that result to the computer. An SQL SELECT query is an example of this, where the steps to return our table's row values are not stipulated. Instead, the final results and filters are described and the language handles all the steps in-between.

I was aware of these programming paradigms, which are sometimes descriptive of computer programs and other times of programming languages, but I had not considered that the base languages of the Web, JavaScript, HTML and CSS, are not all in the same paradigm. Actually, HTML/CSS are declarative languages and JavaScript is imperative. This explains so much about the extra difficulty there is for developers who enter the Web space.

## Story Time

When I started learning Visual Basic in high school it came fairly naturally to me. Writing step-by-step instructions until I had the desired final result was fun and made sense. A couple years later, when I first began writing websites, I found the experience confusing. Where I was always able to fix any problem I discovered in JavaScript, the results of my HTML/CSS decisions were opaque and often unsolvable. Any changes to CSS seemed to break something else and I developed an aversion to the language. I might have concluded that web development wasn't for me, but then I'd see some example of what I wanted on someone else's website and I'd think, "look what can be done, I just don't know how they do it." What I should have said to myself is, "I don't know how to _think_ about it."

My familiarity with CSS has grown with every web project I've built, but that feeling of dissonance never went away. Many solutions to problems I encountered were taken from other's code snippets but, try as I might, I couldn't decompose their work enough to determine how they arrived at that set of instructions. Neither did my colleagues for that matter; it seemed like we all solved problems with other's code snippets. If we didn't hit as many problems with HTML it was only because div use was ubiquitous.

{{< notice type="warning" >}}
It should be briefly noted that CSS wasn't the most friendly language when I first began to use it. Some of my early struggles were caused by missing features for which only magical code was available and not only a dissonant mindset.
{{< /notice >}}

The inelegance of most of our solutions ate at me. If I saw a jumble of JavaScript that looked like those CSS classes we borrowed, I'd realize right away that our solution was poorly designed and refactor it accordingly. No such refactor loop was obvious to me for HTML/CSS.

## Current Day

Gratefully, my feelings about CSS have been radically shifting in the last few months.

I became aware of [Every Layout](https://every-layout.dev/) in November of last year. It was revelatory. For the first time I was seeing a person use HTML/CSS in it's native, declarative way. The approach matched the tool and the outcome was beautiful.

Another tool, the [CUBE CSS](https://cube.fyi/) design philosophy, also equipped me with a critical methodology to refactor CSS I didn't have before. Staring at a swath of CSS classes trying to put my finger on what was wrong with them was exhausting. But I'm now beginning to identify that that fixed margin setting over there, that's because we aren't defining white space at a global level. And that fixed width over on that dropdown, it's because we didn't tell the browser that the options should fit whatever content we put into them with a little extra padding.

## Team Culture

Jeremy takes these concepts another step into daily reality. He insightfully notes that the paradigms grow less from the nature of the programming language itself, but from the culture of the team that uses them. This has certainly been my experience; no amount of writing CSS taught me to think declaratively; it was interaction with others that accomplished that.

Though I agree that the paradigm from which a team approaches their project is a critical ingredient in the final outcome, Jeremy avoids assigning judgment (I don't see why; it's not like we developers ever argue over this stuff 🙄). I do think it follows that, if a programming language is written in a declarative style, an imperative approach will always be going against the grain. It will be dissonant. Therefore it would be superior, and less dissonant, for a team to embrace the dual nature of web development by utilizing imperative design thinking with JavaScript and declarative with HTML/CSS.

(pause to laugh 🙄 😂)

Ok, now that we've all had a laugh at my idealism, Jeremy's response is very much in-tune with how projects actually go. I'll be taking these ideas with me when I'm working with my analytical colleague who, to use Jeremy's illustration, wants to know all the color values. And when I'm trying to solve a problem in CSS, I'll move out a little and ask what the boundaries of my project are. Maybe there's a way to describe the boundaries without imperatively writing out every single case?
