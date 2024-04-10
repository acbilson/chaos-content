+++
author = "Alex Bilson"
date = "2024-04-10 08:04:43"
lastmod = "2024-04-10 08:52:51"
epistemic = "evergreen"
tags = ["software","development","ai","llm"]
+++
In Hrishi Olickel's [LLM Hacker Guide](https://www.youtube.com/watch?v=gsO5V30h-lU) he describes some of the ways his thoughts about the development process have changed since working closely with Large Language Models (LLMs).

## Development Process

Hrishi explores the ways that the development process has changed. He offers two other in-between processes, but below is the traditional and the new AI methodology.

{{< diagram >}}
flowchart LR
	 Write --> Compile --> Run
{{< /diagram >}}

{{< diagram >}}
flowchart LR
	 Chat --> Play --> Loop --> Nest
{{< /diagram >}}

Traditional development is about writing code, compiling it, and running it. Other steps may be included depending on your flavor, such as Test-Driven Development (TDD), but this is the essential way to write software for decades. This is important to remember because the new approach is starkly divergent.

In Hrishi's AI paradigm, a developer spends most of their time interacting with the LLM and only a fraction writing code themselves. He doesn't suggest this necessarily because it's the _best_ development approach but because, with how new and unexplored this technology is, developers need a lot of exposure to the tooling to understand what they can and cannot achieve with it.

Hrishi's [sample project](https://www.youtube.com/watch?v=8w0hUcQSDy8) offers a view into this four-part process.

First, Hrishi gives the LLM some data, in this case Podcast notes, and begins asking questions about the data. He is exploring how the LLM perceives the data, what ambiguities it may have about what it's received (which, if answered, dramatically improve later results), and what he might be able to do with it.

Second, he starts to ask the LLM for some results. Perhaps a Python script that will parse the data in a certain way, or a transformation into HTML or JSON. In playing around with his options, since the LLM can experiment so easily, he could have it spit out a basic layout, then change it to NextJS, then add a UI library in the space of five minutes.

Third, when he's reasonably satisfied that the LLM is outputting the results he wants from his chain of prompts, he can loop the prompts with more data to process batches of content. In his example, to extract and structure hundreds more Podcast notes.

Fourth, he nests all these steps into a single program. This is also part of his recommended methodology as he works; break down the work into subtasks that can be unambiguously achieved by the LLM, then line them up to get the final result. When a step isn't quite working, for example, when a text extraction doesn't handle escape characters, it is much simpler to debug and update that subtask than a larger multi-step prompt.

Hrishi recommends the following distribution of time spent on different activities, at least when building a greenfield project or proof-of-concept.

{{< raw >}}
<table>
<tr>
    <th>Activity</th>
    <th>Time Spent</th>
</tr>
<tr>
    <td>Playing</td>
    <td>60%</td>
</tr>
<tr>
    <td>Prompt Tuning</td>
    <td>20%</td>
</tr>
<tr>
    <td>Input Massaging</td>
    <td>10%</td>
</tr>
<tr>
    <td>Coding</td>
    <td>10%</td>
</tr>
<tr>
    <td>Tooling</td>
    <td>1%</td>
</tr>
</table>
{{< /raw >}}

Hrishi notices that developers familiar with the original paradigm are likely to fall into a pattern of making small, incremental changes with the AI. He urges us to ignore that instinct and instead launch whole new chats. A developer would never consider writing the same program seven times before he gets it right, but the AI can spit out a program in a few seconds.

This AI process is less effective when working on a large, legacy codebase. LLMs aren't able to effectively hold the context necessary to work on a software project of the average size of 50 person company.

## Tips

Hrishi suggests a few tips that he's gathered from months of working with LLMs.

- Use several modalities. Not only text, but also speech-to-text and image processing. You'll be exposed to what the LLM is capable of with different mediums, and people are more likely to fill out explanations when speaking and to be more terse when writing.
- Use structured input and output. Computers work best with structure and everything has some structure. At Hrishi's company they most often use JSON, TypeScript, and SQL queries when interacting with LLMs. Hallucinations are reduced with structured output because they provide expectation guardrails.
- Use the models directly. Tools are being built on top of LLMs to simplify certain kinds of work, but a developer will become much more familiar with the power and limitations of LLMs by interacting with them directly.
- Start with a smaller, cheaper LLM. Hrishi notes that, if you can get a smaller LLM to achieve the results you expect 80% of the time, you can upgrade to a more powerful LLM to achieve better results. The smaller LLM will fail faster and more often, giving you the chance to improve your prompts and your inputs. That refinement is crucial for making a reliable abstraction.
- Most errors are caused by input mistakes, either in the data that's fed into the LLM or the prompt itself. The LLM is rarely the root cause of errors.
- Use many models. LLMs have each been tuned uniquely and are now so different that we'd think of them as different people. Hrishi offers an anecdote that, if one of your employees can't do a task, you don't assume it cannot be done. You offer it to another employee who can.
- As a rule of thumb, aim for approximately the same amount of input to output. Offering the LLM only a little input and asking for lots of output often results in bland, generic results.
- The ways that LLMs can be leveraged is unexplored, but there are three arenas where it shines. First, it's very good at Natural Language Processing (NLP). It can take a loosely structured input and pull out meaning and structure. Second, it is effective at data transformation. Third, it can filter and extract summaries and details from a mass of data. And finally, it is an effective companion for the process of ideation.
