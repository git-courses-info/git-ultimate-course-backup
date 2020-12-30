# ultimate-git-course

NOTE: These notes go together with the cheat sheet that was provided by the course, which also has some extra notes, about some cmds that where missing.

## 5. COLLABORATION

A course from codewithmosh.com

- `origin` is a reference to the cloned repo
- `origin/master` is a remote tracking branch

- git pull
  warning: Pulling without specifying how to reconcile divergent branches is
  discouraged. You can squelch this message by running one of the following
  commands sometime before your next pull:

  - git config pull.rebase false # merge (the default strategy)
  - git config pull.rebase true # rebase
  - git config pull.ff only # fast-forward only

### .9 Storing Credentials

- [Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
- [git-credential-cache](https://git-scm.com/docs/git-credential-cache) Helper to temporarily store passwords in memory  
   -`git config --global credential.helper cache`
- If you’re using a Mac, Git comes with an “osxkeychain” mode, which caches credentials in the secure keychain that’s attached to your system account. This method stores the credentials on disk, and they never expire, but they’re encrypted with the same system that stores HTTPS certificates and Safari auto-fills.
  - `git credential-osxkeychain`
  - `git config --global credential.helper osxkeychain`

### 10. Sharing tags

- Tags are ref's that point to specific points in Git history. Tagging is generally used to capture a point in history that is used for a marked version release (i.e. v1. 0.1). A tag is like a branch that doesn't change. [Tagging](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag#:~:text=Tags%20are%20ref's%20that%20point,branch%20that%20doesn't%20change.)
- By default git doesn't push our `tags` to our repo... So we `git push origin v1.0`
- To delete it: `git push origin --delete v1.0`
- Delete it on the local repo: `git tag -d v1.0`

### 11. Releases

- Use Releases to package your software, along with source code, binary files (e.g. compiled files of the app) and release notes.
- When you create a Release on github, it will be added to the latest commit.

### 12. Sharing Branches

- Similarly to tags, branches are local, private by default.
- Check if a branch has a remote tracking branch: `git branch -vv`

        * feature-1 11eb84c 11. Releases
        main      11eb84c [origin/main] 11. Releases

- Check for remote branches: `git branch -r`

        origin/HEAD -> origin/main
        origin/main

- So the first time you push a branch: `git push -u origin feature-1`
- To delete a branch in remote repo: `git push -d origin feature-1`
- To delete a branch in local repo: `git push -d feature-1`

### 13. Colaboration Workflow

- If you create a branch in the remote repo and then do `git fetch`, and then run `git branch`, you only see `main`,
  because when we run `fetch`, we only get the remote branch. So then we need to create a local one and map it to the remote one.
  - git switch -C feature-1 origin/feature-1
- Remove tracking branches that are deleted on the remote repo: `git remote prune origin`. Or `git fetch --prune`: git fetch --prune is the best utility for cleaning outdated branches. It will connect to a shared remote repository remote and fetch all remote branch refs. It will then delete remote refs that are no longer in use on the remote repository. [Git Prune](https://www.atlassian.com/git/tutorials/git-prune#:~:text=git%20fetch%20%2D%2Dprune%20is,use%20on%20the%20remote%20repository.)

### 14. Pull Requests

- Pull requests let you tell others about changes you've pushed to a branch in a repository on GitHub. Once a pull request is opened, you can discuss and review the potential changes with collaborators and add follow-up commits before your changes are merged into the base branch.
  - **Note: When working with pull requests, keep the following in mind:**
  - If you're working in the shared repository model, we recommend that you **use a topic branch** for your pull request. While you can send pull requests from any branch or commit, with a topic branch you can push follow-up commits if you need to update your proposed changes.
  - When pushing commits to a pull request, don't force push. Force pushing can corrupt your pull request.[docs](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/about-pull-requests).
- Pull requests display diffs to compare the changes you made in your topic branch against the base branch that you want to merge your changes into [docs](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/about-comparing-branches-in-pull-requests).

### 15. Resolving Conflicts

### 16. Issues

- Track all kind of issues like bagfixes, new features, enhancements, or any kind of ideas we want to discuss in the team.

### 18. Milestones

- Use milestones to track the progress of varius issues.
- We can add a banch of issues to a milestone, and then see the progress in that milestone.

### 19. Contributing to open-source Projects

- `Fork` the `repo`, `clone` it, make changes, `push`, when ready make a `pull request`, if ok then maintener will close it.

### 20. Keeping a Forked Repository Up to Date

- Add a new `remote` -call it `upstreem` or `base` with the address of the **original** `repo`. If the forked repo gets behind, you get a notification in github. Fetch the commits from `upstreem` and then push them to the forked repo. Then if there is a branch where we do work, it's a good practice to merge the main branch to that one, in case there are changes that could effect our work. This will reduce coflicts when mainter would want to close our pull request.

### 21. Collaboration using VSCode

### 21. Collaboration in GitKraken

- Options:
- Push a new `branch` to remote... To create a new branch, right click on the last commit of master... Make a commit in VSCode then in GitKraken right click on the new branch and push...
- Push new `tags`.
- Add new remotes (cloud icon on the left).
- Manage pull requests (hub icon on the left). Right click on a branch - `Start a pull request...`

## 5. REWRITING HISTORY

To be able to extract meaningful info out of the history, we need to have a clean history, thus look out for:

- Poor commit messages - Reword them.
- Large commits - Split them.
- Small commits - Squash them to related commits.

Also:

- Drop unwanted commits.
- Modify the content of commits.

**Golder rule:**

- **DON'T REWRITE PUBLIC HISTORY**!
- **DON'T REWRITE PUBLIC HISTORY**!

- e.g.

  - If you modify a commit you've allready pushed in github, git is going to make a new commit (because commits are immutable) so the history will change... Thus if you try to push, it will be rejected.

  ```js
  ! [rejected]        main -> main (non-fast-forward)
  error: failed to push some refs to 'https://github.com/footios/ultimate-git-course.git'
  hint: Updates were rejected because the tip of your current branch is behind
  hint: its remote counterpart. Integrate the remote changes (e.g.
  hint: 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```

  You would need first to merge the origin/main with the local/main, resolve the conficts and then push. It's like trying to push after someone else has pushed to origin. Be careful! Do NOT do a `git push --force`, just to have a linear history. If someone else pushed new code to orign/main, it will be deleted. And even if they haven't... when they will try to push, they would have to pull first and have a non-linear history in their repo. So...

  **DON'T REWRITE PUBLIC HISTORY**!

### 5. Undoing Commits

- `hard reset`
  - `soft`: Removes the commit only. It doesn't touch the staging area or the working directory, so we'll have changes in the staging area.
  - `mixed`: Unstages files, so changes are in the working directory.
  - `hard`: Discards local changes and we have a clean working directory.

### 6. Reverting Commits

- The git revert command can be considered an 'undo' type command, however, it is not a traditional undo operation. Instead of removing the commit from the project history, it figures out how to invert the changes introduced by the commit and appends a new commit with the resulting inverse content. This prevents Git from losing history, which is important for the integrity of your revision history and for reliable collaboration.[pls read more... Git Revert](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert#:~:text=Summary,in%20regards%20to%20losing%20work)
- `git revert `should be used to undo changes on a public branch, and `git reset` should be reserved for undoing changes on a private branch.[pls read more... Resetting, Checking Out & Reverting](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)
- NOTE: After you change something while reverting, and then try to push, if someone else made a commit, your push operation will be rejected with message: `Updates were rejected because the tip of your current branch is behind its remote counterpart.`
- `git revert --no-commit`: By adding `--no-commit` git will figure out the changes that need to be undone, and add the required changes to the staging area.

### 7. Recovering Lost Commits

- `git reflog` to get all the commits. Then `git reset --hard HEAD{#} or ID to get the commit back.
- `git reflog branchName` to get all the commits of a specifiek branch.

### 8. Amending the Last Commit

- In case you want to add something like new code on existing files, or even a new file to the last commit, after you stage the changes, instead creating a new commit, type `git commit --amend`.

### 9. Amending an Earlier Commit

- We need to use **interactive rebasing**. With rebasing, we can replay a bunch of commits on top of another commit. Type `git rebase -i ID`.
- **Reabasing rewrites history**. When you `edit` a commit, all the commits that follow will be recreated... So do NOT use `rebase` for published commits!
- The changes you make in one earier commit get carried on to the next ones.

### 10. Dropping Commits

- Start the rebase operation with the parent of the commit you want to drop.\
  `git rebase -i ID~1 or ID^`

### 11. Rewarding a Commit

- To reward a commit message, use interactive rebasing.\
  In the given options, chose `reword`.

### 12. Re-ordering Commits

- To re-order a commits, use interactive rebasing.\
  Then just move the commit where you want.

### 13. Squashing Commits

- To squash commits, use interactive rebasing.\
   Then chose the `squash` option for all children commits. You may reorder them first if needed.\
  If you want to keep just the name of the parent commit, use `fixup` instead of `squash`.
