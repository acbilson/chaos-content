+++
author = "Alex Bilson"
date = "2023-09-20 09:18:18"
lastmod = "2023-09-20 14:33:35"
epistemic = "sprout"
tags = ["chatgpt","llm","prompt-engineering","ai"]
+++
# Prompt Engineering Tips

1. use delimiters to instruct the model which text to work upon (and which to ignore).
2. request structured output (JSON, CSV, Markdown, etc).
3. offer a way out if the input does not meet certain criteria.
4. give the model more context and supply steps. Reduces errors, especially in deductive reasoning.
5. Specify how the output should look.
6. request that the model complete the solution before supplying an answer. This short-circuits some errors caused by answering before all the text has been processed.

# Best Uses

## Iterative Processes

Jeff's example is taking a CSV data set of books sold. Starting with the need to convert this data to a chart, he requests a popular charting library, then asks for the HTML to present it, and finally requests the model to refine the chart code (color, etc).

Another avenue for iterative use could be transfer of ideas from audio to text. For example, recording my description a problem out loud assists my thinking, but then I have a poorly organized document since speech-to-text tries to match word-for-word what I've said. A model might take that audio dump and convert it to a more organized, proofread version closer to a final product with little effort.

## Summarizing

Offer the model a complex or lengthy online article, chunk of paragraphs or multiple articles and request a summary of the content. Can be personally useful for research, but can also be used to make summaries for a target audience to assist communication.

## Inferring

For example, sentiment analysis. Summarize the emotions exhibited in a paragraph, such as a product review. Can pull data out of a paragraph, like details of a request, and supply a structured result.

## Transformation

Translation, JSON to HTML, text proofreading.

## Expansion

Most debated capability. Can take a small amount of text, like a product idea, and create more content such as a product design or essay.

Tip: specifying a temperature will give the model more room for creativity. A temperature of zero will always respond with the most precise match. A higher temperature results in more varied results. Temperature may only be configured on the API (but it's a cheap per-use license).

(notes from Jeff's Prompt Engineering presentation at the 2023 PTCP IT Conference)

# Poor Uses

It's not wise to use LLM models for research. Asking it about historic information, even that which should be readily available in the public domain, can return hallucinated results. As a test I asked ChatGPT 3.5 questions about the implications of Julius Ceasar's crossing of the Rubicon on the Roman Republic's transition to an Empire. The initial results were reasonably accurate. Then I asked it for artistic expressions of the event and it gave me an authoritative-looking list. However, the first on the list had a correct artist name but, rather than the painting that it described, this artist seems to have produced a bronze statue. ChatGPT seems to have merged a real person with a fictitious work of art or, perhaps, a piece of artwork produced by another artist.
