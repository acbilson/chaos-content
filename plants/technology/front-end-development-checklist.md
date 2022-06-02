+++
author = "Alex Bilson"
date = "2022-03-17"
lastmod = "2022-06-02 14:52:30"
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
  <div>
    <input type=checkbox name=states />
    <label for=states>Alternative data states are handled. Alternate user, different institution type, etc.</label>
  </div>
  <div>
    <input type=checkbox name=auth />
    <label for=auth>Access restricted to only the correct users. Adopt the principle of least privilege.</label>
  </div>
  <div>
    <input type=checkbox name=viewport />
    <label for=viewport>All views of the site have been accounted. Mobile, print, etc.</label>
  </div>

</form>
{{< /raw >}}
