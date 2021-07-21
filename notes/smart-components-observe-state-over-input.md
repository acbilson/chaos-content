+++
backlinks = [
    "/notes/angular-maxims"
]
author = "Alex Bilson"
comments = false
date = "2021-06-11T20:05:50"
epistemic = "seedling"
tags = ["snippet","javascript","software","state","observable","rxjs"]
title = "Smart Components Observe State Over Input"
+++
In component design there are two buckets, smart and dumb components. Smart components are keepers of data models and reactive to changes. Dumb components render explicit data models to views.

Dumb components benefit from the explicit interface created by a clear set of inputs. Smart components, however, can get into a lot of trouble if they depend on mutable inputs for their data models. While it seems easy to add an onChanges() function to child smart components to re-render based on changes to their inputs, this dependency can have unexpected consequences. Smart components respond to changes in their data models, including changing the model. These changes trigger child smart components down the line, who respond even if the change doesn't affect them. Or the change may actually affect their model, but not in a way that's expected.

```
ngOnInit(): void {
  // refreshes when institution, period, or category changes
  const categoryId$ = this.route.params.pipe(map(params => Number(params.categoryId)));
  combineLatest([this.sessionQuery.federalReserveId$, this.sessionQuery.qualitativePeriodId$, categoryId$])
    .subscribe(([fedId, periodId, categoryId]) => this.refreshViewModel(fedId, periodId, categoryId));
}
```

TODO: add current example
