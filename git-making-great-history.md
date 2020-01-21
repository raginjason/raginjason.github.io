# Git: Making Great History

There are many guides on getting started with Git and plenty of resources for people to get their questions answered about this awesome tool, but most of this centers around a more-or-less clean setup. After spending years working with Git in various real world scenarios, I've decided to collect some of my thoughts regarding best practices from the trenches.


## Important Git Features

### Git blame

At the outset, the `git blame` subcommand sounds very accusatory but in fact is a very valueable feature of Git. By default, this will prepend the commit hash/id, author, and date to each line of a file. Optionally you can also pass a revision to `git blame` and it will show you the state of the file in question at that point in time. The `git blame` view of history is important to understand how specific lines of code entered revision history. Part of tailoring revision history should keep this view in mind.

### Git rebase

TODO

## Concepts

### Always consider the "future you"

One of the advantages of revision control is the ability to step back through time to find out what the state of things were at either at a point in time or at a given commit. There are arguments as far as whether commit messages should be imperative or past tense, but that is largely a stylistic question and beyond the scope of this document. The "future you" is you (or your co-worker, or your team member) 6 months from now looking at what you were trying to do today. Ultimately, this is what revision control is for. It's not for "today you". Changing your target audience from "today you" to "future you" will greatly improve the quality of your Git history. 

### Git commit messages

It's one thing to have many "FIXME" commits while working on a specific feature or bug, but by the time the body of work is ready for public eyes it should have proper commit messages. Depending on the project, the definition of "proper" can vary, but typically the commit messages should describe the work that was done and more than likely have a ticket/bug/incident ID in it for cross-reference purposes. Commits of "fixed bug", "typo", "WIP" and the like should all be considered red flags at the time of code review

Commit messages are an opportunity to talk about changes that impacted multiple files atomically. The message should be more oriented towards the bug or feature that is being worked. Often it's important to talk about what was removed as much as what was added.

Commit messages and code comments are different things with different goals. Code comments should not be pseudo code; that's what the code is for. If there is a need for pseudo code, refactor the code so that it's more readable. Sometimes comments should talk about the things that are non-obvious to someone familiar with the language in question. More commonly, they are good for explaining odd requirements such as "We always run on 16 core machines, so partitioning 16 ways" or for explaining why some kind of odd technical solution was chosen. Make all attempts possible for the code to be read without comments. 

### A commit is an atomic body of work

As much as possible, a commit should be treated as an atomic body of work. A good example of this is when moving a function from one file to another. 2 files were impacted, but it's really one body of work. This should be a single commit instead of 2 separate commits. Another example is refactoring a chunk of code into a separate function prior to altering its logic. This is actually 2 bodies of work; 1 to break the chunk of code into a function, 1 to alter the logic. This should be 2 commits, as this will help code review make sense and for `git log` to show reasonable data.

### A branch is a conceptual body of work

A branch should represent a concept. Depending on the branching strategy, that concept could be "fix bug 1234" or it could be "release v2.0 of product". Commits are the building blocks and real implementation of this concept. Attempt to keep the commits on a branch in agreement with the concept of the branch. Avoid the idea of "while the hood is open, might as well do this...". If the concept of the branch can be satisfied by simply changing `FooClass`, don't make changes to `BarClass` because of a need to remove warts from it. The fixes to `BarClass` are independent and should be treated as such.

### Introduce tests in commits prior to code change 

Hopefully the project in question has some amount of automated testing. Attempt to make it so the code does not break the tests. If you are doing [TDD](https://en.wikipedia.org/wiki/Test-driven_development) then you may well create the tests first and then write the code. Commiting the tests up front before any actual code is written is fine. Yes, running tests at that point in history will show tests failing, but this is actually a good thing because it at least suggests that the new tests don't simply return success.

### Dealing with whitespace and other tech debt

In a perfect world, [tech debt](https://en.wikipedia.org/wiki/Technical_debt) would not exist but in fact it does. Ideally, there would be initiatives to remove tech debt. In reality, this seldom, if ever, happens. This means tackling tech debt when working on the next bug or feature. A simple example is code formatting rules. Adding code formatting rules to a codebase that never had them is a massive undertaking, especially if testing isn't thorough. A good way of side stepping this issue is when beginning the branch for the feature/bug in question, perform the whitespace correction as the first commit, then have commits for adding/adjusting tests, then have commits for the actual code change. Why the separate commits? Because combining whitespace changes with actual logic changes makes `git blame` absolutely useless. It will show up as one large commit, the `git diff` output will show the entire file changed even though the change wasn't nessecarily material. Come code review time, having these as separate commits can be a real life saver.

### Managing templates

If there are both templates and applied templates of any kind in the project, don't touch them in the same commit. There should be some amount of commits for the template alterations and one or more commit of the template being applied. This is so that code review and `git history` look reasonable. The business logic change was what happened to the template, this should be the topic of review. What happened to the applied templates is a side effect of the bug or issue.

### Managing lookup/data files

Sometimes there are small lookup or data files in the project. Typically these would be small enough that adding them to a database wouldn't make sense for some reason or another. If the data file supports it i.e. it has a reasonable key, make it policy that the file should be sorted. Over time, this will make `git diff` look more reasonable instead of simply adding records to the end of the file.

### Do not allow old commented code to live

During development, it's reasonable to comment out old code. Perhaps the work in question is a refactor and it's helpful to have it around until the refactor is complete. Some time before the work is complete, that commented code should be removed. If some "future you" wants to see the old version, that's what `git history` is for. Do not muddy the waters by having a changeset that comments out that code in your repository. Remove it so that the history is clear. If it's a refactor, then ideally it would be removed in the same commit that introduces the new code.

### TODO

* git history should look sensible
* Cover the case of splitting a commit into multiple commits with different files
* Cover the case of splitting a commit into multiple commits with 1 file; interactive staging
* Code reviews should look at all commits, that's why it should be broken up
* A fast forward merge is guaranteed to work, make your work fast-forwardable
* comments vs commit messages
