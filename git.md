#### Barrel Development Best Practices


# Git Best Practices
----------------------------
- [Commit Frequency](#commit-frequency)
- [Working with Others](#working-with-others)




## Commit Frequency

- Avoid working all day and then adding an "EOD Commit". Instead, commit every time you have added a meaningful chunk of code. This way, if you need to revert back to an older commit, there are frequent, logical timestamps for you to revert to.
- Give commits a meaningful name (ex. git commit -m "Added BX Slider to homepage")




## Working with Others

When working with other devs, you should never work directly on the Master branch. Generally, it's best to make "feature branches", which can be merged into master as you complete features.  

#### Tips for working on Feature Branches

- From time to time, fast forward your branch (catch it up with Master). This will help to prevent conflicts when you merge your feature into Master.
- When merging a feature into Master, add a pull request on github rather than doing a simple merge. This helps to document the integration and lets other devs comment on the merge.
- Always fast forward your branch right before adding a pull request so that you can resolve conflicts before submitting the PR.
- Delete feature branches after they have been merged into Master.



## Working on an Existing App

Once an app is live, it's a good idea to add a "staging" branch to your repo, and change your workflow to something like:

1) Create a feature branch, add code.
2) Merge your feature into "Staging"
3) Push the Staging code to the staging server for testing.
4) Confirm that everything works.
5) Merge your feature into "Master".
6) Push Master code to the production server and delete feature branch.

This way, if you are in the middle of testing a feature and something more important comes up (a bug on production), you can fix the issue without having to remove your feature code from Master (because only production-ready code is ever merged to Master).