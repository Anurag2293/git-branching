# Git Branching
Notes from learngitbranching.js.org

## Main 

### A. Introduction Sequence

1. **Introduction to Git Commits** \
   A commit in a git repository records a snapshot of all the (tracked) files in your directory. It's like a giant copy and paste, but even better! \
   - *To Create changes*: `git commit`
   
2. **Branching In Git** \
   Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. \
   - *To Create new branch*: `git branch <newBranchName>` 
   - *To Checkout a branch*: `git checkout <name>` 
   - *To Create & Checkout a new branch*: `git checkout -b [yourbranchname]`

3. **Merging in Git** \
   Merging in Git creates a special commit that has two unique parents. A commit with two parents essentially means "I want to include all the work from this parent over here and this one over here, and the set of all their parents." \
   - *To merge B2 into current branch*: `git merge B2` 

4. **Rebase Introduction** \
   The second way of combining work between branches is rebasing. Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else. \
   - *To rebase B2 onto current branch*: `git rebase B2` 
