+++
author = "Alex Bilson"
date = "2023-03-02"
lastmod = "2023-03-03 11:14:48"
+++

## Purpose

Our team advises banks and credit unions based on hundreds of public data points and ratios. For the best advice, we need the most accurate, current data. The regulatory bodies that publish this data operate on a quarterly schedule, pushing out massive volumes of numbers every three months. Banks and Credit Unions, however, publish corrections to their reports, called call reports, throughout the quarter which are not reflected in the bulk quarterly data. What's more, large chunks of this data are presented, not in easily parsable CSV files, but in PDFs. We needed to scan thousands of PDFS and load data from multiple web endpoints into our system on a daily basis to keep up with the numbers.

A non-technical person at our firm had hacked together a PDF parser to simplify their own analysis. Each call report has hundreds of account fields configured differently on nearly every page. When the call report format changed and broke their parser, they decided it was time to give up on the project. But the work they'd done was invaluable, and so it was given to our team to make into a robust solution. And, since I'm the only one proficient in Python (and Rust, and Go...) it became my project. Yes!

## Design

What the original owner accomplished in hacking [PDFMiner](https://pypi.org/project/pdfminer/) and [Selenium](https://pypi.org/project/selenium/) together was a huge win, but the design needed a rehaul. He'd used whatever he had at his disposal to identify account cells, all the way to selecting them by the font style. For a non-programmer it was brilliant, but in my hands we could make it much more resilient.

### Pixel Distance

One of the primary challenges was identifying an account with it's value. Cells are different sizes, styles, and orientations. How to associate these cells without writing a brittle parser?

{{< notice >}}
It's worth noting that a brittle parser is exactly what I wrote first. I did just enough to parse the new call report design so I could run the parser for the latest batch and get data into our system. Business needs come before architecture excellence.
{{< /notice >}}

After designing a parser with page-specific configuration I landed on an even better calculation that utilized pixel coordinates inside a threshold. Certain pages needed a wider or narrower threshold because of their table design, but the default could handle 80% of the cases. Not only for the new call report, but even for call reports four years ago. Wanna see it?

{{< highlight python >}}
def find_value_by_word(
    elements: List[T], word: CallReportWord, max_dist_x=100, max_dist_y=20
) -> CallReportWord:
    word_values = [CallReportWord(el) for el in elements]
    for word_value in word_values:
        if (
            (abs(word.mid.y - word_value.mid.y) < max_dist_y)
            and (word_value.mid.x - word.mid.x < max_dist_x)
            and (word_value.mid.x - word.mid.x > 0)
        ):
            return word_value
    return None
{{< /highlight >}}

{{< notice >}}
I've had colleagues who are skittish of any programming language without strong typing, but I enjoy the progressive nature of an optional typing system like Python or JavaScript. Adding typing to the original project makes it so much easier to maintain, but not being constrained by typing made exploratory development so much simpler.
{{< /notice >}}

I calculated the midpoint of each cell by taking the difference between the lower-left and upper-right coordinates. This was crucial because sometimes the text would be aligned to the left, top, bottom, or right of the cell. A midpoint calculation made for a much more consistent overall result.

### Multiple Engines

Despite my efforts to genericise the parser to handle the huge variety of table layouts, the variance between different call report pages and designs requires a degree of configuration.

For each call report edition I create a parser that extends my base class. I have six different page parsing strategies that I can map per page. When there are differences in a page's parsing strategy for a new call report edition, I can simply override the base method.

{{< highlight python >}}
 def __init__(self):
	  self.effective_date = date(2020, 6, 30)
	  self.pages_start_on_index = 3
	  self.page_map = {
			"skip": [0, 1],
			"dates": [2],
			"normal": [
				 3,
				 4,
				 5,
				 6,
				 7,
				 8,
				 9,
				 10,
				 11,
				 12,
				 13,
				 14,
				 16,
				 17,
				 18,
				 19,
				 21,
				 22,
				 24,
			],
			"both": [20],
			"horizontal": [23],
			"special": [15],
	  }
{{< /highlight >}}

### Error Handling

My first foray into the Rust programming language happened around the time I began to refine the call report parser project. Rust's method style of returns that consist of both value an optional error became a useful pattern to bubble up errors and warnings without crashing the parser. My parsing functions and many of their dependencies return Tuples with the result object and a list of error messages. Python already has great destructuring, so it was a natural fit into my project.

As an example, here's the parsing strategy for a page that's so different from the others it deserved its own strategy. You can see that it returns a list of errors, and that the method it calls, `get_amounts_by_account` also returns an errors list. These error messages get aggregated at a higher level and optionally printed based on logging configuration.

{{< highlight python >}}
 def parse_schedule_g(
	  self, page: PDFPage, page_num: int
 ) -> Tuple[List[CallReportItem], List[str]]:
	  """
	  Schedule G page parsing algorithm. Sets x-axis threshold 3x wider to account for amount values
	  more than one column width to the left. Gets the correct amount by selecting the left-most figure.
	  """
	  accounts = self.get_accounts(page)
	  amount_sets, errors = self.get_amounts_by_account(
			page,
			accounts,
			max_dist_x=self.cfg.x_threshold * 3,
			max_dist_y=self.cfg.y_threshold,
			take_leftmost=True,
	  )
	  if len(accounts) != len(amount_sets):
			errors.append(
				 f"On page {page_num} found {len(accounts)} accounts but {len(amount_sets)} amounts"
			)
	  return (
			[
				 CallReportItem(
					  page=page_num,
					  acct=x[0].text,
					  input_str=x[1].text,
					  match_distance=x[0].match_distance,
				 )
				 for x in amount_sets
			],
			errors,
	  )
{{< /highlight >}}


{{< notice >}}
While all this parser code was both fun and functional, less than a year after I rewrote the whole parser the organization that publishes this data exposed a public API and we could retrieve the data from a JSON payload instead. That's progress for ya.
{{< /notice >}}

## Highlights

### Selenium

One of the unexpected roadblocks I encountered was a memory leak. The parser would be humming along but after a dozen rounds of parsing or so it would hang indefinitely, be automatically scaled down, start up again, and repeat. A typical job might restart each instance of the parser twenty times. Huh.

I used a native Python [profiler](https://docs.python.org/3/library/profile.html) to dig further into what was happening. It was my first time using such a tool but, to my delight, it took less than half a day to identify the culprit! Hmmm, it's a method in the Selenium package?

{{< notice >}}
I wish that I'd written down my steps while I was figuring this out because it was a cool exercise. Now I don't remember exactly what I encountered nor do I have snippets of the stack traces. Ah, well.
{{< /notice >}}

Skipping a lot of steps that I can't remember, I began to dig through the Selenium public source code. And what do you know, I found the issue! It turned out that the web driver [opens a process](https://github.com/SeleniumHQ/selenium/blob/trunk/py/selenium/webdriver/firefox/firefox_binary.py#L96) to log messages to a file. Even if it's output is `/dev/null` this process is opened and data dumped into it. The trouble is, that process is never closed and so it began to eat up memory.

Thankfully Python doesn't have restrictive private properties like a strongly typed language, so I was able to reach into the internals and close the log file handler. Below is a wrapper I created to ensure that instances of the driver were properly disposed. All the original configurations are present in case you find something amiss or need a robust example.

#### Usage

This was my first attempt at creating a disposable object that can be loaded with `using`. Nifty.

{{< highlight python >}}
 with DriverManager(
	  download_path=download_path,
	  firefox_exe_path=app.config["FIREFOX_EXE"],
	  gecko_driver_exe_path=app.config["GECKO_DRIVER_EXE"],
 ) as driver:
	  result = site.scrape(driver=driver)
{{< /highlight >}}

#### DriverManager

And here's the original code.

{{< highlight python >}}
import os
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.firefox.service import Service
from selenium.webdriver import Firefox


class DriverManager:
    """wraps a selenium driver instance
    attrs:
        download_path (str): the folder location that driver downloads will be placed inside
        firefox_exe_path (str): the path to the Firefox executable
        gecko_driver_exe_path (str): the path to the Gecko Driver executable
    """

    def __init__(
        self,
        download_path: str,
        firefox_exe_path: str,
        gecko_driver_exe_path: str,
    ):
        self.download_path = download_path

        # Setup the firefox webdriver
        service = Service(executable_path=gecko_driver_exe_path, log_path=os.devnull)

        options = Options()
        options.headless = True
        options.binary = firefox_exe_path
        options.set_preference("browser.download.folderList", 2)
        options.set_preference("browser.download.manager.showWhenStarting", False)
        options.set_preference("browser.download.dir", download_path)
        options.set_preference("download.prompt_for_download", False)
        options.set_preference(
            "browser.helperApps.neverAsk.saveToDisk", "application/pdf"
        )
        options.set_preference("pdfjs.disabled", True)
        options.set_capability("marionette", True)

        self.driver = Firefox(options=options, service=service)

    def __enter__(self) -> Firefox:
        return self.driver

    def __exit__(self, exception_type, exception_val, trace):
        # closes file handler manually to fix memory leak
        self.driver.binary._log_file.close()
        self.driver.quit()
{{< /highlight >}}

{{< notice >}}
I'd meant to submit a GitHub issue back when I first discovered this memory leak. Never too late though; you can find the issue <a href="https://github.com/SeleniumHQ/selenium/issues/11730">here</a>.
{{< /notice >}}

### Dockerfile

This project was one of the first that made good use of my discoveries on how to {{< backref src="/plants/technology/shrink-your-python-dockerfile" >}}.
