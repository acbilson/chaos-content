+++
aliases = ["/posts/multi-repository-madness/"]
category = "technology"
comments = true
date = "2020-03-11"
description = "In which Alex shares about his experience building a feature across eight interconnected Git repositories."
tags = ["powershell", "git"]
title = "Repository Madness"
addendum = ["I say, \"our velocity,\" but one of my takeaways from this process was how important it is to get buy-in for a tool before I invest. I thought that, if I made the script useful and user-friendly, everyone would adopt it. In reality, each member used portions of the script at best."]
[featuredImage]
  alt = "Terminal Output"
  large = "https://raw.githubusercontent.com/acbilson/ps-git-sync/aim/images/action-branch.png"
  small = "https://raw.githubusercontent.com/acbilson/ps-git-sync/aim/images/action-branch.png"
+++
A few months ago, three colleagues and I added a new feature to a legacy .NET application. One of the technical challenges we faced was the distributed nature of the application's codebase.

## Background

The code history was originally stored in [Subversion](https://en.wikipedia.org/wiki/Apache_Subversion) (SVN), but the organization had shifted its active repositories from SVN to Git in early 2019. One of the carry-overs from the original was a multi-repository structure. To build the app to which we'd add features, each local machine needed to clone eight repositories. Except for one master repo, each repository had dependencies in at least one other and sometimes more than one. Some contained mostly business implementations, others that were mostly object definitions, and yet others that were primarily web services.

For example, if I were working on a service controller and wanted to add a new object with an interface, I'd have to create its interface in a separate repo, then write the implementation in my current repo. Certain objects were implemented in a second repo and defined in a third.

It wasn't long into the job before we realized that changes in the system might have unforseen results. There are three we overcame with technolory: exceptions from mismatched repos, a forest of irrelevant uncommited changes, and mistakes in syncing dozens of feature branches.

## Mismatched Repositories

Every time a repository was updated, each team member might spend a half-day tracking down mysterious errors. An exception would point to a method in one repository, but when the developer tracked the error to its source he'd discover that the error had originated, not in the repo that threw the error, but in another repo entirely. Eventually we caught on that any error we'd see after a git pull was likely caused by a mismatch in changes between repos. There were two common culprits.

First, if a developer didn't pull updates for every repository then one or more repositories would become out-of-sync. This might cause a build error, but sometimes it would only cause a runtime error.

Second, if a developer failed to commit AND push every change relevant to their feature work, then no amount of repo synchronization would resolve the errors. Often the developer wouldn't know they'd missed a change because of the next painful experience.

## A Forest of Irrelevant Changes

A developer's changes were nearly impossible to identify in a forest of file creations and updates caused by the build process. These changes were relevant only to the product's release, not to the current feature.

Every time the code was built, each repository would increment the app version in multiple assembly version files. For the repos which contained front-end code, a flood of HTML would be generated by a precursor to the Razor view engine and copied to the master view repository. Any given repo would have multiple terminal buffers full of changes after each call to git status.

You might rightly assess that .gitignore files would alleviate this problem, and they did after I created them, but remember that these are changes across eight repositories. Any time a developer needed to verify which files have been changed, he'd need to run `git status` in eight separate directories. Since most developers check for changes before every commit, and most features require many commits, there's a ton of room for error.

## Branch Sync Mistakes

Our team operated on a single working branch per repository. Each dev would also have one or more feature branches in every repo, sometimes several if a completed feature was waiting for code review.

For example, if I needed to make changes to three files to implement a story and each file was in a different repo, I would need a feature branch in each. If four devs each have two feature branches open across eight repositories, how many potential branch iterations could I have checked out locally? `(4*2)^8`=over sixteen million! And that's assuming that everyone's branches have been pushed to the remote repository and up-to-date on every local machine, which wasn't always the case.

## Resolution

The developers who worked on this code every day had personal processes which made their work proficient despite these challenges. For our team to be productive from the first commits, we took a few actions.

The first was to add .gitignore files to exclude the massive number of generated files from the local build process. We didn't receive authorization right away, so I wrote a quick and dirty PowerShell script to explicitly restore or delete the irrelevant files. This script became irrelevant once a configured .gitignore file was added to our branch.

With the forest of changes tamed, I built a PowerShell script to automate many Git tasks that reduced the likelihood of branch mistakes and mismatched repos. My script could switch to the same branch across every repository, pull the latest changes everywhere, or print a comprehensive file status. The script didn't ensure we'd never see any of these issues again, but it did speed up our velocity{{< superset 1 "#addendum" >}} and cut down on mistakes.

Are you interested in how I implemented the PowerShell script, or need a copy for your own? Check out the source code for the [ps-git-sync script](https://github.com/acbilson/ps-git-sync.git)!