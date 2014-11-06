### Barrel Development Best Practices

# Git Best Practices
- [Commit Frequency](#commit-frequency)
- [Working with Others](#working-with-others)
- [Working on an Existing App](#working-on-an-existing-app)
- [Github Issues](#issues)
- [Git Quick Reference](#quick-reference)

## Commit Frequency

- Avoid working all day and then adding an "EOD Commit". Instead, commit every time you have added a meaningful chunk of code. This way, if you need to revert back to an older commit, there are frequent, logical timestamps for you to revert to.
- Give commits a meaningful name (ex. git commit -m "Added BX Slider to homepage")

## Working with Others

Assume your code will be inherited by another person. You should **never** work directly on the master branch. It's best practice to make "feature branches," which can be merged into master as you complete features.  

#### Tips for working with [Feature] Branches

- Avoid using really long-winded branch names; instead use the most descriptive words, all lowercase, and joined by hyphens. If you're using long branch names, it becomes a pain to review.
- When merging a feature into Master, **add a pull request on github** rather than doing a simple merge. This helps to document the integration and lets other devs comment on the merge. You can perform localized test merges before issuing a pull request to ensure a smooth merge.
- Always fast forward your branch and/or rebase from the most recent merge point before adding a pull request so that you can resolve conflicts before submitting the PR.
- Delete feature branches after they have been merged into Master.

## Working on an Existing App

Once an app is live, it's a good idea to add a "staging" branch to your repo, and change your workflow to something like:

1. Create a feature branch, add code.  
2. Merge your feature into "Staging"  
3. Push the Staging code to the staging server for testing.  
4. Confirm that everything works.  
5. Merge your feature into "Master".  
6. Push Master code to the production server and delete feature branch.  

This way, if you are in the middle of testing a feature and something more important comes up (a bug on production), you can fix the issue without having to remove your feature code from Master (because only production-ready code is ever merged to Master).

## Issues

Github Issues are a great way to keep track of bugs and feature requests. Issues can also be linked to Commits and Pull Requests, which leaves a nice paper trail for anybody trying to track progress on the project.

To link an issue, simply use the special [issue syntax](https://help.github.com/articles/closing-issues-via-commit-messages) within a commit message. When your commit is merged into the Master branch, the issue will close automatically.

For Example, given the issue `#21 - Usernames should link to the user's profile`, you could add the folliwing commit:

```
git add .
git commit -m "fixes #21, usernames now link to user's profile"
```

Once merged into Master, the keyword _"fixes #21"_ lets Github know to close issue #21 and add a reference to this commit.

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
