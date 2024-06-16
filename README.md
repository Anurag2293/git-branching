# Git Branching
Notes from [learngitbranching.js.org](https://learngitbranching.js.org)

## Main 

### A. Introduction Sequence

1. **Introduction to Git Commits** \
   A commit in a git repository records a snapshot of all the (tracked) files in your directory. It's like a giant copy and paste, but even better! 
   - *To Create changes*: `git commit`
   
2. **Branching In Git** \
   Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. 
   - *To Create new branch*: `git branch <newBranchName>`
   - *To Create new branch at a Commit*: `git branch <newBranchName> <commit>` (commit can be absolute or relative)
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
     
### C. Moving Work Around

1. **Git Cherry-pick** 
    ```
    git cherry-pick <Commit1> <Commit2> <...>
    ```
   - `git cherry-pick` will plop down a commit from anywhere in the tree onto HEAD (as long as that commit isn't an ancestor of HEAD).
   - It's a very straightforward way of saying that you would like to copy a series of commits below your current location (HEAD).

2. **Git Interactive Rebase** 
   - We can use interactive rebasing to review a series of commits you're about to rebase. 
   - *All interactive rebase means Git is using the rebase command with the -i option.*
   - If you include this option, git will open up a UI to show you which commits are about to be copied below the target of the rebase. It also shows their commit hashes and messages, which is great for getting a bearing on what's what.
   - The real git interactive rebase allows you to do things like squashing (combining) commits, amending commit messages, and even editing the commits themselves.
   - Eg. To Interact with 4 Commits above HEAD
   ```
   git rebase -i HEAD~4
   ``` 

### D. A Mixed Bag

1. **Grabbing Just 1 Commit** 
   - Here's a development situation that often happens: I'm trying to track down a bug but it is quite elusive. In order to aid in my detective work, I put in a few debug commands and a few print statements.
   - All of these debugging / print statements are in their own commits. Finally I track down the bug, fix it, and rejoice.
   - Only problem is that I now need to get my bugFix back into the main branch. If I simply fast-forwarded main, then main would get all my debug statements which is undesirable.
   - We need to tell git to copy only one of the commits over. This is just like the levels earlier on moving work around -- we can use the same commands:
      - `git rebase -i`
      - `git cherry-pick`

2. **Juggling Commits**
   #### Situation
   Here's another situation that happens quite commonly. You have some changes (newImage) and another set of changes (caption) that are related, so they are stacked on top of each other in your repository (aka one after another).
   <br><br>
   The tricky thing is that sometimes you need to make a small modification to an earlier commit. In this case, design wants us to change the dimensions of newImage slightly, even though that commit is way back in our history!!
   
   #### Solution
   We will overcome this difficulty by doing the following:

    - We will re-order the commits so the one we want to change is on top with git rebase -i
    - We will git commit --amend to make the slight modification
    - Then we will re-order the commits back to how they were previously with git rebase -i
    - Finally, we will move main to this updated part of the tree to finish the level (via the method of your choosing)

3. **Juggling Commits #2**
   #### Situation
   As you saw in the last level, we used rebase -i to reorder the commits. Once the commit we wanted to change was on top, we could easily --amend it and re-order back to our preferred order.
   <br><br>
   The only issue here is that there is a lot of reordering going on, which can introduce rebase conflicts.

   ![Start](https://github.com/Anurag2293/git-branching/assets/83635812/15d9cfa5-2bbf-4cf6-87c3-7faa219479c0) --> ![Goal](https://github.com/Anurag2293/git-branching/assets/83635812/204ddf8c-dfbb-48e9-ad47-1b1bfc549882) 

   #### Solution
   Let's look at another method with `git cherry-pick`. 
   - `git checkout main`
   - `git cherry-pick C2`
   - `git commit --amend`
   - `git cherry-pick C3`
   
4. **Git Tags**
   - Branches are easy to move around and often refer to different commits as work is completed on them. Branches are easily mutated, often temporary, and always changing.
   - Git tags (somewhat) permanently mark certain commits as "milestones" that you can then reference like a branch.
   - *If you leave the `commit` off, git will just use whatever HEAD is at.*
      ```
      git tag <tagName> <commit>
      ```

5. **Git Describe**
   - `git describe` is used to describe where you are relative to the closest "anchor" (aka tag).
   - Git describe can help you get your bearings after you've moved many commits backwards or forwards in history; this can happen after you've completed a git bisect (a debugging search) or when sitting down at the computer of a coworker who just got back from vacation.
   - Git describe takes the form of:
      ```
      git describe <ref>
      ```
      Where `<ref>` is anything git can resolve into a commit. If you don't specify a `ref`, git just uses where you're checked out right now (`HEAD`).

   - The output of the command looks like:
     ```
     <tag>_<numCommits>_g<hash>
     ```
      Where `tag` is the closest ancestor tag in history, `numCommits` is how many commits away that `tag` is, and `<hash>` is the hash of the commit being described.


### E. Advanced Topics

1. **Rebasing Multiple Branches**
   #### Situation
   - We want the commits to all be in sequential order. So this means that our final tree should have C7' at the bottom, C6' above that, and so on, all in order.
   - ![image](https://github.com/Anurag2293/git-branching/assets/83635812/958ba4b8-4d01-4a66-94ea-fb0760f17e48) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/fcf4bb8e-68b6-4d2d-a492-cc47991b0771)
  
   #### Solution
   - `git rebase main bugFix`
   - `git rebase bugFix side`
   - `git rebase side another`
   - `git rebase another main`

2. **Multiple Parents**
   #### Specifying Parents
   - Like the ~ modifier, the ^ modifier also accepts an optional number after it.
   - Rather than specifying the number of generations to go back (what ~ takes), the modifier on ^ specifies which parent reference to follow from a merge commit. Remember that merge commits have multiple parents, so the path to choose is ambiguous. 
   - Git will normally follow the "first" parent upwards from a merge commit, but specifying a number with ^ changes this default behavior.
   - These modifiers can be chained together.

   #### Situation
   ![image](https://github.com/Anurag2293/git-branching/assets/83635812/88ae517c-24c5-4163-a4de-41fc5f820645) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/a3bec1aa-c3fc-4d9c-b5ed-73eea8dac5e1)

   #### Solution
   - `git branch bugWork main^^2^`
   
3. **Branch Spaghetti**
   #### Situation
   - Here we have main that is a few commits ahead of branches one two and three. For whatever reason, we need to update these three other branches with modified versions of the last few commits on main.
   - Branch one needs a re-ordering of those commits and an exclusion/drop of C5. Branch two just needs a pure reordering of the commits, and three only needs one commit transferred!
   - ![image](https://github.com/Anurag2293/git-branching/assets/83635812/a8fa80c3-6436-41c1-a24e-aa46ff406bd4) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/21f90e49-6f3a-4678-b70f-6a553d708d75)

   #### Solution
   - `git checkout one`
   - `git cherry-pick C4 C3 C2`
   - `git checkout two`
   - `git cherry-pick C5 C4 C3 C2`
   - `git branch -f three C2` 

<br>
<br>
<br>

## Remote

### A. Push & Pull -- Git Remotes!

1. **Clone Intro**
   - `git clone` is used to create local copies of remote repositories (from github for example).
   - Makes connection between local and remote repository.

2. **Remote Branches**
   #### Git Remote Branches
   - The first thing after running `git clone` is that a new branch appeared in our local repository called `origin/main`. This type of branch is called a remote branch; remote branches have special properties because they serve a unique purpose.
   - Remote branches reflect the state of remote repositories (since you last talked to those remote repositories). They help you understand the difference between your local work and what work is public -- a critical step to take before sharing your work with others.
   - Remote branches have the special property that when you check them out, you are put into detached HEAD mode. Git does this on purpose because you can't work on these branches directly; you have to work elsewhere and then share your work with the remote (after which your remote branches will be updated).
   - *To be clear: Remote branches are on your local repository, not on the remote repository.*

   #### What is origin/?
   - Remote branches also have a (required) naming convention -- they are displayed in the format of:
     ```  
     <remote name>/<branch name>
     ```
   - Hence, if you look at a branch named `origin/main`, the branch name is `main` and the name of the remote is `origin`.
   - This is so common that git actually sets up your remote to be named `origin` when you git clone a repository.

   `When we checkout a remote branch, we are in Detached HEAD State. The remote branch won't be updated when we commit changes to HEAD`

3. **Git Fetchin'** \
   To fetch data from a remote repository -- the command for this is conveniently named git fetch.

   #### What fetch does
   - git fetch performs two main steps, and two main steps only. It:
      - downloads the commits that the remote has but are missing from our local repository, 
      - updates where our remote branches point (for instance, o/main)
   - `git fetch` essentially brings our local representation of the remote repository into synchronization with what the actual remote repository looks like (right now).
   - `git fetch` usually talks to the remote repository through the Internet (via a protocol like http:// or git://).

   #### What fetch doesn't do
   - `git fetch`, however, does not change anything about your local state. It will not update your main branch or change anything about how your file system looks right now.
   - You can think of running git fetch as a download step.

4. **Git Pullin'**
   - Once you have new commits available locally, you can incorporate them as if they were just normal commits on other branches. This means you could execute commands like:
       `git cherry-pick o/main`
       `git rebase o/main`
       `git merge o/main`
   - `git pull` === `git fetch` + `git merge <remoteBranch>`
   - `git pull --rebase` === `git fetch` + `git rebase <remoteBranch>`

5. **Faking Teamwork**
   - `git fakeTeamwork`: Not a real command. Just used here to simulate teamwork in remote repository.

6. **Git Pushin'**
   - `git push` is responsible for uploading your changes to a specified remote and updating that remote to incorporate your new commits.
   - *note -- the behavior of git push with no arguments varies depending on one of git's settings called push.default. The default value for this setting depends on the version of git you're using, but we are going to use the upstream value in our lessons. This isn't a huge deal, but it's worth checking your settings before pushing in your own projects.*

7. **Diverged History**
   - To avoid ambiguity where history has diverged, git doesn't allow you to push your changes. It actually forces you to incorporate the latest state of the remote before being able to share your work.
   - You need to do is base your work off of the most recent version of the remote branch.
   - Ways to do it:
     - `git rebase`: move your work via rebasing
       ```
       git fetch
       git rebase origin/main
       git push
       ```
     - `git merge`: Although git merge doesn't move your work (and instead just creates a merge commit), it's a way to tell git that you have incorporated all the changes from the remote. This is because the remote branch is now an ancestor of your own branch, meaning your commit reflects all commits in the remote branch.
       ```
       git fetch
       git merge origin/main
       git push
       ```
   - We can use `git pull` and `git push` as a shorthand technique for achieving above.

8. **Locked Main**
   #### Remote Rejected
   - If you work on a large collaborative team it's likely that main is locked and requires some Pull Request process to merge changes. If you commit directly to main locally and try pushing you will be greeted with a message similar to this:
   - `! [remote rejected] main -> main (TF402455: Pushes to this branch are not permitted; you must use a pull request to update this branch.)`

   #### Why was it rejected!
   - The remote rejected the push of commits directly to main because of the policy on main requiring pull requests to instead be used.
   - You meant to follow the process creating a branch then pushing that branch and doing a pull request, but you forgot and committed directly to main. Now you are stuck and cannot push your changes.

   #### The solution
   - Create another branch called feature and push that to the remote. Also reset your main back to be in sync with the remote otherwise you may have issues next time you do a pull and someone else's commit conflicts with yours.
   - ![image](https://github.com/Anurag2293/git-branching/assets/83635812/351163f7-4e7b-420f-8132-dd2fce08cdff) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/8f6d2786-9497-49ae-9f92-5f02579d3da2)
   - ```
     git reset --hard origin/main
     git checkout -b feature C2
     git push origin feature
     ```


### B. To Origin And Beyond -- Advanced Git Remotes! 

1. **Push Main!**
   #### Merging feature branches
   - It's common for developers on big projects to do all their work on feature branches (off of main) and then integrate that work only once it's ready. This is similar to the previous lesson (where side branches get pushed to the remote), but here we introduce one more step.
   - Some developers only push and pull when on the main branch -- that way main always stays updated to what is on the remote (o/main).
   - So for this workflow we combine two things:
     - integrating feature branch work onto `main`, and
     - pushing and pulling from the remote

   #### Refresher on how to update main and push work
   ![image](https://github.com/Anurag2293/git-branching/assets/83635812/9a913f58-4afa-4dda-ace8-059ff46d61b9) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/f59ea3f3-39eb-47b7-bb1c-93f287ba348e)

   ````
   git pull --rebase
   git push
   ````

   #### Situation
   ![image](https://github.com/Anurag2293/git-branching/assets/83635812/05862051-330a-4a0e-881a-058432671edd) ==> ![image](https://github.com/Anurag2293/git-branching/assets/83635812/3004d8a4-9727-4c89-876f-df91b6bedcba)

   #### Solution
   ````
   git fetch
   git rebase o/main side1
   git rebase side1 side2
   git rebase side2 side3
   git rebase side3 main # Fast Forward
   git push
   ````

2. **Merging with Remotes**
   

   

