+++
author = "Alex Bilson"
date = "2020-02-10"
lastmod = "2021-12-30 10:40:19"
epistemic = "evergreen"
tags = ["azure", "powershell", "cli"]
+++
My wife and I share a grocery list so that either of us could pick up the week’s groceries. For common items, we get into a routine: pick up milk, bread, eggs. But when our needs deviate and we need a specific type of bread or a specific brand of cornbread, there’s a lot more information needed to choose the correct items. For instance, we need to specify the store where the item is sold, what brand, what type, and what quantity.

Software engineer that I am, I got to thinking about whether I could use machine learning to help accomplish more complex grocery shopping.

I determined Microsoft Azure Computer Vision API as a potential resource because of its ability to accomplish a range of valuable image processing tasks. I identified three tasks that could be applied to my family’s grocery list challenge, including:

- Analyze Image – to determine brand and type of food
- Tag Image – to categorize foods, e.g. meat, vegetable, fruit
- Recognize Text – to read the display information, such as name, price, and weight

## First Steps

To begin, I first needed to verify that the API would accomplish my goals. After I registered a free Azure account, I drafted a PowerShell module to simplify my requests to the Computer Vision API and obtain immediate feedback.

The following PowerShell commands were created:

- Request-AnalyzeImage
- Request-TagImage
- Request-RecognizeText
- Request-CreateThumbnail

## Initial Results

I began a test phase with stock images found on the internet. I initially chose two food images, a can of marinara sauce and a box of cereal.

My first tests returned mixed results. Neither the rounded sides of the marinara sauce container nor the fancy text affected the text recognition, which was a win, but the API was unable to identify the Trader Joe’s brand.

The cereal box was even more surprising. While the text was clear, the tags indicated that the API was ‘seeing’ a bowl on a table, as opposed to a cereal box with an image of a bowl and table. Because the API struck out with the Trader Joe’s brand, I ran a similar test on a t-shirt image of a more universal brand to see what results I might return. Brand identified as Google. Success!

Now, it was time to go grocery shopping and collect real-life test data.

## Further Analysis

To supply a high-quality test sample, I took 40 photographs on my last grocery run. The results deviated widely. After my initial test results demonstrated that the Trader Joe’s brand was unidentifiable, I abandoned brand identification in favor of image tagging and text recognition.

Image tagging returned odd results. While Computer Vision did identify Gala apples, with “fruit;food;apple;outdoor” all receiving confidence scores above 75%, Granny Smith apples returned “fruit;food;diet food;natural foods.”

A more structured item, wine, suffered the same result. A bottle of Contadino returned “bottle;wine;text,” but a second bottle of Mbali returned “convenience store, text, soft drink, indoor, bottle, store, scene.” The skewed results were most likely because of the complexity of the labels and the environment.

Text recognition fared better but had similar trouble with the complex environment. Computer Vision picked up the text of many sizes, many fonts, and many slants. The trouble lay with all the noise. For example, the picture of Gala apples returned many words taken from the tiny stickers on each apple, not only the larger sign. I was impressed with how accurate Computer Vision interpreted small print and odd fonts with few mistakes, but it consistently left out the cent symbol. This did lead me to consider another option.

The Computer Vision API struggled to tag grocery items, but it performed remarkably well with text. What if I read the receipt instead? After loading up the picture, the results were spectacular. The text came through with no apparent errors and, because the receipt is a structured document, I used the boundingBox property to filter individual parts of the receipt.

With a regular expression filter, I sorted the results into titles and amounts, then matched the title with the amount using a fuzzy y-axis coordinate. A little tweaking and I had the item titles matched with the amounts — an unexpected win!

## Conclusion

When I began this project, I was unsure whether the Computer Vision API would meet my family’s grocery shopping needs. With a little testing, I confirmed that:

- Image analysis isn’t a good fit for grocery store items due to the unstructured, complex nature of the environment
- Image tagging does not regularly return the identification tags I need for a grocery list
- Text recognition is both nimble and accurate

To view the ComputerVision PowerShell module and run the tests for yourself, both the PowerShell files and images are available on my computer-vision-psmodule Github repository.

Overall, if I were to build a fully-featured grocery store application, I would leverage the Computer Vision API to translate grocery receipts into structured data. The text recognition accurately captures a lot of insight, such as quantity and price, which could answer budget questions as well as build a centralized grocery list.

(original post on SPR's website [here](https://spr.com/grocery-shopping-computer-vision)).

