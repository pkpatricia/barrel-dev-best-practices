### Barrel Development Best Practices

# Git Best Practices
- [Commit Message Conventions](#commit-message-conventions)
- [Commit Frequency](#commit-frequency)
- [Working with Others](#working-with-others)
- [Working on an Existing App](#working-on-an-existing-app)
- [Github Issues](#issues)
- [Git Quick Reference](#quick-reference)

## Commit Message Conventions
1. Commit header (first line) should succinctly describe changes in high-level changes. Think: Fix, Add, Remove, Change.
2. Commit body (subsequent lines) should be imperative present tense, and break out into more low-level details.

####Verbose (bad):

**[BR-137] Site down message on error handler / [BR-57] Auto save & save on timeout**
* This commit requires a migration for the time_remaining column to add
to the essays table

####Succinct (good):

**Fixes BR-137 and BR-57** or **Add auto-save and site down error handler**
* Add auto-save on timeout
* Add auto-save functionality
* Commit requires a migration for the time_remaining column to add to the essays table

--
Links for reference:
- See github [article](https://help.github.com/articles/closing-issues-via-commit-messages/#keywords-for-closing-issues) on closing issues in commit messages.
- See jira [article](https://confluence.atlassian.com/display/Cloud/Processing+JIRA+issues+with+commit+messages#ProcessingJIRAissueswithcommitmessages-Thecommands) on processing jira tickets with commit messages. Note at the very least, you can easily attach commits to a jira ticket by including the ticket reference in your commit message.

## Commit Frequency

- Avoid working all day and then adding an "EOD Commit". Instead, commit every time you have added a meaningful chunk of code. This way, if you need to revert back to an older commit, there are frequent, logical timestamps for you to revert to.
- Give commits a meaningful name (ex. git commit -m "Add BX Slider to homepage").
- **Try to make more commits in your branch.**
The more you commit, the easier it is to rebase or merge or cherry-pick individual commits. You might consider separating your models, controllers, migrations in separate smaller commits.
- **Make sure your branch is limited to one feature.** 
It's ok if the branch has only one commit for even the smallest feature, but it makes it easier if we need to have someone else pickup the work for that specific issue. In this case, ensure the features being committed to your branch match the branch's stated purpose.
- **Name your feature branch.**
Another benefit of naming your branch according to feature is that everyone will know what your branch contains. If your feature covers a range of files and other features, make sure your branch reflects that by indicating "various-edits" or "bundle-fixes" or, if it spans several fixes for a general issue, use something like "fix-timing-issues" instead of "bobs-tickets" where someone must review each commit and the files changed to get a better idea of what's added, removed, changed, or fixed. If you are bundling edits, this is best achieved by creating a branch from a common ancestor and merging all the relevant feature branches you wish to commit to the bundled branch.

## Working with Others

Assume your code will be inherited by another person. You should **never** work directly on the "master" branch. It's best practice to make "feature branches", which can be merged into "master" as you complete features.  

#### Tips for working with [Feature] Branches

- Avoid using really long-winded branch names; instead use the most descriptive words, all lowercase, and joined by hyphens. If you're using long branch names, it becomes a pain to review.
- When merging a feature into Master, **add a pull request on github** rather than doing a simple merge. This helps to document the integration and lets other devs comment on the merge. You can perform localized test merges before issuing a pull request to ensure a smooth merge.
- Always fast forward your branch and/or rebase from the most recent merge point before adding a pull request so that you can resolve conflicts before submitting the PR.
- Delete feature branches after they have been merged into "master".

#### Avoid erroneous or duplicate commits when merging
By refraining from cyclical or redundant merges, you can ensure you don't reintroduce bugs that were previously removed or wipe out concurrent changes from other developers. 
- Avoid merging the same branches: `Merge remote-tracking branch 'origin/fix-tickets' into fix-tickets`
- Avoid merging master into your branch: `Merge branch 'master' into fix-tickets`
  - Instead try to rebase your branch onto master.
  - Alternatively, try creating a new branch and cherry-pick your good commits.
- Initially perform `git fetch --all` to ensure all local branches are synced with the remote. 
- If you see any inconsistencies, you can review, rebase, refactor commits into a new branch by cherry-picking, or get clarification from the project maintainer.

## Working on an Existing App

Once an app is live, it's a good idea to add a "staging" branch to your repo, and change your workflow to something like:

1. Create a feature branch, add code.  
2. Merge your feature into "staging"  
3. Push the "staging" code to the staging server for testing.  
4. Confirm that everything works.  
5. Merge your feature into "master".  
6. Push "master" code to the production server and delete feature branch.  

This way, if you are in the middle of testing a feature and something more important comes up (a bug on production), you can fix the issue without having to remove your feature code from "master" (because only production-ready code is ever merged to 
"master").

## Issues

Github Issues are a great way to keep track of bugs and feature requests. Issues can also be linked to Commits and Pull Requests, which leaves a nice paper trail for anybody trying to track progress on the project.

To link an issue, simply use the special [issue syntax](https://help.github.com/articles/closing-issues-via-commit-messages) within a commit message. When your commit is merged into the "master" branch, the issue will close automatically.

For Example, given the issue `#21 - Usernames should link to the user's profile`, you could add the folliwing commit:

```
git add .
git commit -m "fixes #21, usernames now link to user's profile"
```

Once merged into "master", the keyword _"fixes #21"_ lets Github know to close issue #21 and add a reference to this commit.

## Quick Reference
Push to branch 'master' on remote 'origin':
```
git push origin master
```
Pull from branch 'master' on remote 'origin':
```
git pull origin master
```
Retrieve commit hash of current commit, last commit, five commits prior:
```
git rev-parse HEAD
git rev-parse HEAD~
git rev-parse HEAD~5
```
Stage/add all untracked files:
```
git add --all
```
Commit all staged/added files:
```
git commit --all
```
Checkout an existing branch 'staging':
```
git checkout staging
```
Retrieve/update all remote tracking branches:
```
git fetch --all
```
Create a new branch 'test':
```
git branch test
```
Delete branch 'test':
```
git branch -D test
```
Add a remote 'origin':
```
git remote add origin https://github.com/barrel/barrel-dev-best-practices.git
```
List all remotes verbosely:
```
git remote -v
```
