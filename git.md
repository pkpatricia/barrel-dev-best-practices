#### Barrel Development Best Practices

# Git
----------------------------
- [Working with Others](#working-with-others)



## Commit Frequency

- Avoid working all day and then adding an "EOD Commit". Instead, commit every time you have added a meaningful chunk of code. This way, if you need to revert back to an older commit, there are frequent, logical timestamps for you to revert to.
- Give commits a meaningful name (ex. git commit -m "Added BX Slider to homepage")


## Working with Others

When working with other devs, you should never work directly on the Master branch. Generally, it's best to make "feature branches", which can be merged into master as you complete features.  

### Tips for working on Feature Branches

- From time to time, fast forward your branch (catch it up with Master). This will help to prevent conflicts when you merge your feature into Master.
- When merging a feature into Master, add a pull request on github rather than doing a simple merge. This helps to document the integration and lets other devs comment on the merge.
- Always fast forward your branch right before adding a pull request so that you can resolve conflicts before submitting the PR.
- Delete feature branches after they have been merged into Master.