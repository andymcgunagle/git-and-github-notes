# Git & GitHub Notes

## Git Notes

### Init & Status

- Do not initialize a git repository inside an existing git repository (use `git status` to verify you're not currently inside an existing repo before running `git init`).

### Adding/Staging & Unstaging

- `git add <file>`: Stage specific updated files in the *working directory* to the *staging area* before committing them to the git *repository*.

- `git add .`: Stage all updated files in the working directory at once.

- `git restore --staged <file>`: Unstage specific files from the staging area.

- `git reset`: Unstage all files from the staging area.

### Restoring

- `git restore .` or `git restore <file>`: Discard changes to all files or specific files in the working directory. If a path is tracked but does not exist in the restore source, it will be removed to match the source.

- Note the difference in actions when the `--staged` flag is not used.

### Committing

- `git commit -m "My commit message..."`: The commit message should summarize the changes in that commit.

- Try to keep each commit focused on a single thing. This makes it easier to undo changes later on and can also make your code easier to review.

- [`git config --global core.editor "code --wait"`](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config): Sets VS Code as your your core.editor and enables you to use `git commit` (without the `-m` flag) to open a new file in VS Code and easily compose longer commit messages there.

- `git commit -a -m "My commit message..."`: The `-a` flag automatically stages files that have been modified and deleted (without the need for `git add`). 
  - Note that new files you have not told Git about are not tracked - you'd need to include them with `git add <file>`.

### Logging

- `git log --oneline`: Shows the first line of commit messages and a shorter snippet of the 40-byte hexadecimal commit object name.

### Amending Commits

- `git commit --amend`: Rather than making a brand new separate commit, you can "redo" the previous commit using the `--amend` flag. 
  - Only works for the previous commit. 
  - Will enable you to update the previous commit message.
  - If you forgot to stage files, use `git add <file>` first.

### Ignoring files

- Create a file called **.gitignore** in the root of a repository.
  - `.DS_Store` would ignore files named **.DS_Store**.
  - `folderName/` will ignore an entire directory.
  - `*.log` will ignore any files with the **.log** extension.

### Branching

- `HEAD` is a pointer that refers to the current "location" in your git repository - it points to a particular branch reference.

- `git branch`: View your existing branches.
  - `*` will indicate the branch you are currently on.
  - Adding the `-v` flag will provide a bit more info on each branch.  

- `git branch <branch-name>`: Creates a new branch based on the current HEAD. 
  - This command does not switch HEAD to the new branch - it just creates the new branch.
  - Branch names should not include spaces.

- `git switch <branch-name>`: Once you have created a new branch, this will switch HEAD to it.
  - `git checkout <branch-name>` can also be used to switch you to a different branch. `git checkout` also does a ton of other things, so `git switch` was made for simplicity.
  - You'll need to commit or stash your changes before you switch to a divergent branch.

- `git switch -c <branch-name>`: One-liner to create and switch to the newly created branch.

- `git branch -d <branch-name>`: Deletes a branch.
  - Cannot delete the branch the HEAD is currently on.

- `git branch -m <new-branch-name>`: Rename a branch.
  - Must be on the branch you want to rename.

### Merging branches

- Incorporate changes from one branch into another.
  - Branches are merged, not specific commits.

- Note that you'll always merge to the current HEAD branch.
  - So, first you'll point the HEAD at the receiving branch using `git switch <receiving-branch-name>`.
  - Then you'll merge the other branch into that branch using `git merge <branch-being-merged-name>`.
  - Branches are not deleted when merged. If they're no longer needed, you can delete them manually after changes are merged using `git branch -d <branch-name>`.

- If there wasn't additional work on the receiving branch and the merge is simply updating the receiving branch to include newer upstream revisions from the branch being merged, then the merge is called a *fast-forward merge*. The pointer is simply moved forward.

- If there has been additional work on the receiving branch, then git performs a *merge commit* - you end up with a new commit on the receiving branch and git will prompt you for a message.

#### Resolving Merge Conflicts

1. Open up the file(s) with merge conflicts.
2. Edit the file(s) to remove the conflicts - keep content from one or both of the branches.
3. Remove the conflict "markers" in the document.
4. Add your changes, then make a commit.

### Comparing Changes

- `git diff`: Without any flags will list all the changes in our working directory that are not staged. The header in the output that's generated will be in the following format:
  - Which files are being compared (typically just the same file over time): `diff --git a/README.md b/README.md`
  - File metadata (usually not important): `index 59489d0..393256e 100644`
  - Changes in file **a** will be indicated with a minus sign: `--- a/README.md`
  - Changes in file **b** will be indicated with a plus sign: `+++ b/README.md`
  - "lineChunkBegins, numberOfLinesInChunk" in file **a** (minus sign) and file **b** (plus sign): `@@ -100,4 +100,8 @@`

- `git diff HEAD`: Lists *all* changes in the working tree since your last commit (staged and unstaged changes).
  - Can add the file name to view changes in a specific file.

- `git diff --staged` or `git diff --cached`: Lists the changes between the staging area and the last commit (shows all the changes that would be included if you were to commit at that moment).
  - Can add the file name to view changes in a specific file.

- `git diff branch1 branch2` or `git diff branch1..branch2`: List the changes between the tips of branch1 and branch2.

- `git diff commit1 commit2` or `git diff commit1..commit2`: List the changes between the tips of commit1 and commit2.
  - commit1 and commit2 would be the commit hashes of the commits in question. 
  - You can use the shortened commit hash displayed with `git log --oneline`.
  - The output will be easier to read if commit1 is older and commit2 is newer.

### Stashing

- When switching branches, you may encounter the following scenario:
  - "error: Your local changes to the following files would be overwritten by checkout:"
  - "Please commit your changes or *stash* them before you switch branches."

- `git stash` or `git stash save`: Takes all uncommitted changes (staged and unstaged) and stashes them. This cleans the working tree and allows you to freely switch to other branches.
  - You can stash multiple changes onto the stack of stashes. They will all be stashed in the order you added them.
  - `git stash list`: Shows the list of stashes.
  - `WIP` stands for "work in progress".

- `git stash pop`: Removes the most recently stashed changes in your stash and re-applies them to your working copy.

- `git stash apply`: Applies whatever is stashed away without removing it from the stash. Useful if you want to apply stashed changes to multiple branches. This can potentially create merge conflicts that can be resolved in the typical fashion.
  - You can use `git stash apply stash@{2}`, for example, to apply a particular stash from the stash list.

- `git stash drop stash@{2}`: Delete a particular stash by specifying the stash ID number.

- `git stash clear`: Deletes all stashes.

### Undoing Changes & Time Traveling

- `git checkout <commit-hash>`: Jump back to a previous commit. 
  - This will *detach the HEAD* (the HEAD will be pointing at a specific commit rather pointing at the branch pointer as it typically does).
  - Use `git switch <branch-name>` to reattach the HEAD. You can also use `git switch -` to restore the previous HEAD position.
  - You can also create a new branch starting from that previous commit using `git switch -c <new-branch-name>`, for example.
  - `git checkout HEAD~1` is an alternative syntax for jumping back to previous commits relative to a particular commit. `HEAD~1`, for example, refers to the commit before the commit HEAD is currently pointing at.

- `git checkout HEAD <filename(s)>` or `git checkout -- <filename(s)>`: Discard any changes in a specific file, reverting back to the HEAD.
  - `git restore <filename(s)>` can do the same thing.

- `git reset <commit-hash>`: Reset the repo back to a specific commit. This will get rid of the commits that came after the commit you're resetting to.
  - The commits will be removed, but the changes in the working directory will remain.
  
- `git reset --hard <commit-hash>`: Gets rid of the commits and the associated changes that came after the commit you're resetting to.

- `git revert <commit-hash>`: Reverses/undoes the changes made after a specific commit but creates a new commit rather than resetting to the commit.
  - Will prompt you to enter a commit message.
  - Preserves the history of the commit, which makes it easier to reconcile changes when collaborating.

## GitHub Notes

### Cloning

- `git clone <url>`: Copy a repository from GitHub to your local machine.

### SSH Key

- With SSH keys, you can connect to GitHub without supplying your username and personal access token at each visit.

- [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### Remotes

- `git remote` or `git remote -v`: View any existing remotes for your repository.

- "origin" is just a conventional remote name - it's the default remote name when you clone a GitHub repo. You can change it to whatever you like, but most people leave it.

- `git switch <remote-branch-name>` or `git checkout --track <origin, typically>/<remote-branch-name>`: By default only one branch will be set up locally when you clone a repo to your machine. If the repo has multiple remote branches, this command creates a new local branch from an existing remote branch of the same name and sets it to track the remote branch.

- `git branch -r`: The `-r` flag causes the remote-tracking branches to be listed.

### Pushing

- `git push <remote-name> <remote-branch-name>`: Push a specific local branch up to the remote.

- You can push multiple branches up to GitHub.

- Local branches and remote branches are distinct - they can have the same name and content, but technically they're separate entities. 

- Usually you'll be pushing a local branch up to remote branch of the same name, but `git push <remote-name> <local-branch-name>:<remote-branch-name>` would allow you to push a local branch to a remote branch with a different name.

- The `-u` flag in a command such as `git push -u origin main` sets the *upstream* of the branch we're pushing. Once the upsteam for a branch is set, you can use the `git push` shorthand to push the current branch to the upstream.

### Fetching

- Fetching allows you to download changes fom a remote repo into your local repo, but those changes will not be automatically integrated into your working files.

- `git fetch <remote-name>`: Fetches branches and history from a specific remote repository. It only updates remote tracking branches, not the local branches or the working directory on your machine.

- `git fetch <remote-name> <branch-name>`: Fetches a specific branch from a remote repository.

- Take a look at changes you've fetched using `git checkout <remote-name>/<branch-name>`.

### Pulling

- Pulling allows you to retrieve changes from a remote repo, but unlike fetching it updates the HEAD with whatever changes are retrieved from the remote.
  
- `git pull`: Essentially `git fetch` + `git merge`. Pulls changes from the remote branch tracking the local branch you're currently on.

- `git pull <remote-name> <branch-name>`: Just like with git merge, it matters where we run this command from - whatever branch we run it from is where the changes will be merged into.
  - This more specific command becomes more important when there are multiple remotes.

- Pulls can result in merge conflicts.

- It's a good practice, when collaborating on a project, to pull down changes before pushing your work to a remote repo.




