# Git & GitHub Notes

## Git Notes

https://git-scm.com/docs

- Do not initialize a git repository inside an existing git repository (use `git status` to verify you're not currently inside an existing repo before running `git init`).

### Adding/Staging & Unstaging

- `git add <file>`: Stage specific files in the *working directory* that have been updated to the *staging area* before committing them to the git *repository*.

- `git add .` or `git add --all`: Stage all files in the working directory that have been updated.

- `git restore --staged <file>`: Unstage specific files from the staging area.

- `git reset`: Unstage all files from the staging area.

### Committing

- `git commit -m "My commit message..."`: The commit message should summarize the changes in that commit.

## GitHub Notes