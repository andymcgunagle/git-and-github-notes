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

### Committing

- `git commit -m "My commit message..."`: The commit message should summarize the changes in that commit.

- Try to keep each commit focused on a single thing. This makes it easier to undo changes later on and can also make your code easier to review.

- [`git config --global core.editor "code --wait"`](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config): Sets VS Code as your your core.editor and enables you to use `git commit` to open a new file in VS Code where it will be easier to compose longer commit messages.

### Restoring

- `git restore .` or `git restore <file>`: Discard changes to all files or specific files in the working directory. If a path is tracked but does not exist in the restore source, it will be removed to match the source.

## GitHub Notes