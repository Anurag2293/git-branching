# Git Branching
Notes from learngitbranching.js.org

## Main 

### A. Introduction Sequence

1. **Introduction to Git Commits** \
   A commit in a git repository records a snapshot of all the (tracked) files in your directory. It's like a giant copy and paste, but even better! 
   - *To Create changes*: `git commit`
   
2. **Branching In Git** \
   Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. 
   - *To Create new branch*: `git branch <newBranchName>` 
   - *To Checkout a branch*: `git checkout <name>` 
   - *To Create & Checkout a new branch*: `git checkout -b [yourbranchname]`

3. **Merging in Git** \
   Merging in Git creates a special commit that has two unique parents. A commit with two parents essentially means "I want to include all the work from this parent over here and this one over here, and the set of all their parents." 
   - *To merge B2 into current branch*: `git merge B2` 

4. **Rebase Introduction** \
   The second way of combining work between branches is rebasing. Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else. 
   - *To rebase B2 onto current branch*: `git rebase B2` 


### B. Ramping Up

1. **Detach yo' HEAD** \
   HEAD is the symbolic name for the currently checked out commit -- it's essentially what commit you're working on top of. \
   <br>
   HEAD always points to the most recent commit which is reflected in the working tree. Most git commands which make changes to the working tree will start by changing HEAD. \
   <br>
   Normally HEAD points to a branch name (like bugFix). When you commit, the status of bugFix is altered and this change is visible through HEAD.
   <br><br>
   `Detaching HEAD just means attaching it to a commit instead of a branch.`
   <br>
   - *To Checkout a Commit C1*: `git checkout C1`
  
2. **Relative Refs (^)** \
    Use `git log` to see commit history and hashes. \
    With relative refs, you can start somewhere memorable (like the branch bugFix or HEAD) and work from there. \
   <br>
    ***Types of Simple Relative Refs:***
      - Moving upwards one commit at a time with `^` \
        Eg: `git checkout HEAD^`
      - Moving upwards a number of times with `~<num>` \
        Eg: `git checkout HEAD~4`
  
3. **Relative Refs #2 (~)** \
   ***Branch Forcing***
   One of the most common ways to use relative refs is to move branches around. You can directly reassign a branch to a commit with the -f option. So something like:
   ```
   git branch -f main HEAD~3
   ```
   moves (by force) the main branch to three parents behind HEAD.
   <br><br>
   *`Note: In a real git environment git branch -f command is not allowed for your current branch.`*

4. **Reversing Changes in Git** \
   Reversing changes in Git has both a low-level component (staging individual files or chunks) and a high-level component (how the changes are actually reversed.
   <br><br>
   There are two primary ways to undo changes in Git --
   1. `git reset`
      - `git reset` reverses changes by moving a branch reference backwards in time to an older commit.
      - In this sense you can think of it as "rewriting history;" git reset will move a branch backwards as if the commit had never been made in the first place.
      - Eg. `git reset HEAD~1`
   2. `git revert`
      - While resetting works great for local branches on your own machine, its method of "rewriting history" doesn't work for remote branches that others are using.
      - In order to reverse changes and share those reversed changes with others, we need to use git revert. Let's see it in action.
      - Eg: `git revert HEAD` 
   
   
