# Git & GitHub Notes

## Git Notes

### Git Docs

https://git-scm.com/docs

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

- `git diff HEAD`: Lists *all* changes in the working tree since your last commit (staged and unstanged changes).
  - Can add the file name to view changes in a specific file.

- `git diff --staged` or `git diff --cached`: Lists the changes between the staging area and the last commit (shows all the changes that would be included if you were to commit at that moment).
  - Can add the file name to view changes in a specific file.

- `git diff branch1 branch2` or `git diff branch1..branch2`: List the changes between the tips of branch1 and branch2.

## GitHub Notes
