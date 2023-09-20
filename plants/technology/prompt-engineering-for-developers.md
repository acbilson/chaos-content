+++
author = "Alex Bilson"
date = "2023-09-20 09:18:18"
lastmod = "2023-09-20 10:09:04"
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

## Summarizing

Offer the model a complex or lengthy online article, chunk of paragraphs or multiple articles and request a summary of the content. Can be personally useful for research, but can also be used to make summaries for a target audience to assist communication.

## Inferring

For example, sentiment analysis. Summarize the emotions exhibited in a paragraph, such as a product review. Can pull data out of a paragrah, like details of a request, and supply a structured result.

## Transformation

Translation, JSON to HTML, text proofreading.

## Expansion

Most debated capability. Can take a small amount of text, like a product idea, and create more content such as a product design or essay.

Tip: specifying a temperature will give the model more room for creativity. A temperature of zero will always respond with the most precise match. A higher temperature results in more varied results. Temperature may only be configured on the API (but it's a cheap per-use license).

(notes from Jeff's Prompt Engineering presentation at the 2023 PTCP IT Conference)
