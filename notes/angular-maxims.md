+++
author = "Alex Bilson"
comments = true
date = "2021-06-09T21:06:30"
tags = ["software","axiom","angular","design"]
title = "Angular Maxims"
+++
This is an incomplete list of maxims I've gleaned while working on Angular development.

- {{< backref "/notes/favor-identifier-over-index" "Favor Identifier Over Index" >}}
- {{< backref "/notes/simplify-loop-with-iterator-swap" "Simplify Loop With Iterator Swap" >}}
- {{< backref "/notes/organize-subscription-chains-with-pipes" "Organize Subscription Chains With Pipes" >}}

I may add a maxim about using observables. They are extremely helpful; however, the observable.subscribe() pattern hides whether the call is expected to return once or multiple times. It's important to distinguish when there will be a single update and when there will be a series. Series observables usually belong in the onInit() method and drive component behavior when they receive dispatches. One-time observables are merely getting data. The methods to interact with each are different.

For example, one-time uses Ramda's forkJoin to resolve all observables, but combineLatest when updates are expected. Sometimes a combineLatest may include both kinds of observables, but the user doesn't know which (if any) might have multiple dispatches.
