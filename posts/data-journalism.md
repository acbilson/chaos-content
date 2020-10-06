+++
category = "technology"
title = "Visualizing COVID Deaths"
date = "2020-11-11"
description = "In which Alex combines two amazing discoveries in data journalism"
categories = ["technology"]
tags = ["journalism","visualization","datasette"]
[featuredImage]
  large = ""
  small = ""
  alt   = ""
+++
Did you know there's a mature tool to display and inspect sqlite databases via your browser?

And did you know that there are **thousands** of data sets published across the globe that have been collected by [Open Data Inception](https://opendatainception.io/)?

We're going to use the tool, [datasette](https://github.com/simonw/datasette), and a subset of open source data published by the Cook County Medical Examiner [COVID-19 Related Deaths](https://datacatalog.cookcountyil.gov/Public-Safety/Medical-Examiner-Case-Archive-COVID-19-Related-Dea/3trz-enys) to grasp how COVID-19 has affected Cook County.

Before we begin, it's worth noting that the tool Cook County uses to house this data, [Socrata](https://dev.socrata.com/), (by [Tyler Technology](https://www.tylertech.com/Platform-Technologies.html)), has its own visualization technology. If you don't want to get technical with these tools, you can do your own examination [right here](https://datacatalog.cookcountyil.gov/d/3trz-enys/visualization).

## Additional Tools

- injestion [Socrata2SQL](https://docs.datasette.io/en/stable/ecosystem.html#socrata2sql)
- map visualization [Spatialite](https://docs.datasette.io/en/stable/spatialite.html)
- visualization [Vega](https://docs.datasette.io/en/stable/ecosystem.html#datasette-vega)

I chose to visualize COVID because the effect is often hidden from view. There are dozens of other interesting projects just waiting for us to discover. Here's a paper published by the City of Evanston to inspire you and I - [2020 Adopted Budget](https://data.cityofevanston.org/stories/s/tu52-urjb).

Thanks to [Tom MacWright](https://macwright.com/2020/10/01/recently.html) for the datasette reference, and thanks to the datasette creator, Simon Willison, for using the Open Data Inception Project in your [datasette demonstration](https://www.youtube.com/watch?v=pTr1uLQTJNE)!

## Caveats

I've copied these notices from the COVID-19 source data. In case you don't follow the links, I want to be sure these are visible. This data was collected from the site on 10/2/2020.

> This filtered view contains information about COVID-19 related deaths that occurred in Cook County that were under the Medical Examiner’s jurisdiction.This view was created by looking for "covid" in any of these fields: Primary Cause, Primary Cause Line A, Primary Cause Line B, Primary Cause Line C, or Secondary Cause.

> Not all deaths that occur in Cook County are reported to the Medical Examiner or fall under the jurisdiction of the Medical Examiner. The Medical Examiner’s Office determines cause and manner of death for those cases that fall under its jurisdiction. Cause of death describes the reason the person died. This dataset includes information from deaths starting in August 2014 to the present, with information updated daily.
