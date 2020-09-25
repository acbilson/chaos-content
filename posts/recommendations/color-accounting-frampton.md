+++
date = "2020-04-22"
categories = ["business"]
description = "A book recommendation for Frampton and Robilliard's book, Color Accounting"
dropcap = false
tags = ["accounting", "ledger-cli"]
title = "Recommend: Color Accounting"
citations = ["Frampton, Peter and Robilliard, Mark. Color Accounting: The new graphical system that makes understanding accounting easy and quick. Accounting Comes Alive International. 2014"]

[featuredImage]
  alt = "Color Accounting Book Cover"
  large = "https://www.coloraccounting.com/uploads/2/4/8/0/24801950/9675057.jpeg?445"
  small = "https://www.coloraccounting.com/uploads/2/4/8/0/24801950/9675057.jpeg?445"

+++

If you'd asked me at the start of my MBA program what courses I'd enjoy most, accounting would not have made the top five. This book and an engaged professor (thank you Clarke!) launched accounting to my top three.

Frampton and Robilliard's [Color Accounting](https://www.coloraccounting.com/books.html) revolutionized the way I conceive of the flow of capital. Their imagery and definitions exposed me to the fascinating business story that the movement of capital tells and how double-entry accounting documents that story with precision and simplicity.

If you have any responsibility for a budget, this book will equip you to understand the essence of accounting and the accounting reports you need to evaluate your business. If you keep a home budget and find that basic tools aren't giving you a confident view of your spending and financial forecast, this is your introduction to a better way. It's the best primer I've discovered for double-entry accounting ([Frampton and Robilliard](#citations)).

# Tool Suggestion - ledger-cli

When I'd finished the book, I wanted to practice double-entry accounting for my own budget. I built a framework for plaintext budgeting with .NET Core services for text file CRUD operations and an AngularJS web frontend. My work is published on Github at [acbilson/plaintext-budget](https://github.com/acbilson/plaintext-budget). I completed the framework for the first Plaintext Budget version before I discovered ledger-cli. It has everything I've needed and more to operate a complete and semi-automated double-entry accounting workflow.

After you've read the book a couple of times, practice double-entry accounting with the robust Plaintext double-entry accounting CLI tool [ledger-cli](https://www.ledger-cli.org/). Here's a mildly adjusted example taken from my own ledger:

```
; baby wipes and a gift for Amie
1/22 * OnlineStore.com
  Expenses:Home                                            $25.12
  Expenses:Gift                                            $32.24
  Assets:MyBank:Checking                                  -$57.36
```
