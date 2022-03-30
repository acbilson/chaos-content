+++
author = "Alex Bilson"
date = "2022-03-17"
lastmod = "2022-03-30 15:32:07"
epistemic = "sprout"
tags = ["checklist"]
+++
In other fields than software development, checklists ensure a series of complex steps are correctly completed. I wonder why that hasn't caught on in my domain? Anyways, here's my first attempt at a front-end pull request checklist.

{{< raw >}}
<form class=simple>
  <div>
    <input type=checkbox name=descriptive />
    <label for=descriptive>Code is descriptively named or commented</label>
  </div>
  <div>
    <input type=checkbox name=logs />
    <label for=logs>A few useful logs are added.</label>
  </div>
  <div>
    <input type=checkbox name=unused />
    <label for=unused>Unused import statements and console logs are removed.</label>
  </div>
  <div>
    <input type=checkbox name=memory />
    <label for=memory>Memory leaks are resolved. Examples include: hanging subscriptions, etc.</label>
  </div>
</form>
{{< /raw >}}
