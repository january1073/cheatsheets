# Git & GitHub Cheatsheet

## Configuration (Run Once)

| Action                        | Command                                            |
| ----------------------------- | -------------------------------------------------- |
| Set name                      | `git config --global user.name "Your Name"`        |
| Set email                     | `git config --global user.email "you@example.com"` |
| Set default branch to `main`  | `git config --global init.defaultBranch main`      |
| Set VS Code as default editor | `git config --global core.editor "code --wait"`    |
| Check all settings            | `git config --list`                                |

## Starting a Repository

| Action                      | Command                    |
| --------------------------- | -------------------------- |
| Initialize Git in directory | `git init`                 |
| Clone remote repository     | `git clone <url>`          |
| Clone into specific folder  | `git clone <url> <folder>` |

## Basic Workflow

| Action                | Command                       |
| --------------------- | ----------------------------- |
| Check status          | `git status`                  |
| Track new file        | `git add <file>`              |
| Track all changes     | `git add .`                   |
| Unstage file          | `git restore --staged <file>` |
| Discard local changes | `git restore <file>`          |
| Commit staged changes | `git commit -m "message"`     |
| Amend last commit     | `git commit --amend`          |

## Working with Commits

| Action                    | Command                     |
| ------------------------- | --------------------------- |
| View log                  | `git log`                   |
| One-line summary          | `git log --oneline`         |
| Show changes of commit    | `git show <commit>`         |
| Revert commit (safe undo) | `git revert <commit>`       |
| Reset to previous commit  | `git reset --hard <commit>` |

## Branching

| Action                | Command                |
| --------------------- | ---------------------- |
| List branches         | `git branch`           |
| Create branch         | `git branch <name>`    |
| Switch branch         | `git switch <name>`    |
| Create and switch     | `git switch -c <name>` |
| Delete branch (local) | `git branch -d <name>` |
| Force delete branch   | `git branch -D <name>` |

## Merging & Rebasing

| Action                     | Command                                   |
| -------------------------- | ----------------------------------------- |
| Merge branch into current  | `git merge <branch>`                      |
| Rebase onto other branch   | `git rebase <branch>`                     |
| Abort rebase               | `git rebase --abort`                      |
| Continue rebase            | `git rebase --continue`                   |
| Resolve conflicts manually | Edit files, then `git add`, then continue |

## Remote Repositories (e.g., GitHub)

| Action                       | Command                           |
| ---------------------------- | --------------------------------- |
| Add remote                   | `git remote add origin <url>`     |
| Check remotes                | `git remote -v`                   |
| Push to remote (first time)  | `git push -u origin main`         |
| Push to remote (after setup) | `git push`                        |
| Pull changes                 | `git pull`                        |
| Fetch remote updates         | `git fetch`                       |
| View remote branches         | `git branch -r`                   |
| Rename remote                | `git remote rename origin github` |
| Remove remote                | `git remote remove origin`        |

## GitHub SSH Setup (Optional)

| Action                       | Command / Location                           |
| ---------------------------- | -------------------------------------------- |
| Generate SSH key             | `ssh-keygen -t ed25519 -C "you@example.com"` |
| Copy public key to clipboard | `cat ~/.ssh/id_ed25519.pub`                  |
| Add to GitHub (UI)           | GitHub → Settings → SSH & GPG keys           |
| Test connection              | `ssh -T git@github.com`                      |
| Use SSH URL                  | `git@github.com:user/repo.git`               |

## Stashing Changes

| Action                   | Command                    |
| ------------------------ | -------------------------- |
| Save uncommitted changes | `git stash`                |
| List stashes             | `git stash list`           |
| Apply last stash         | `git stash apply`          |
| Apply and drop           | `git stash pop`            |
| Drop specific stash      | `git stash drop stash@{1}` |

## Tags

| Action                 | Command                             |
| ---------------------- | ----------------------------------- |
| Create lightweight tag | `git tag v1.0.0`                    |
| Create annotated tag   | `git tag -a v1.0.0 -m "Version 1"`  |
| List tags              | `git tag`                           |
| Push tags              | `git push origin --tags`            |
| Delete local tag       | `git tag -d v1.0.0`                 |
| Delete remote tag      | `git push origin :refs/tags/v1.0.0` |

## Cleaning Up

| Action                              | Command         |
| ----------------------------------- | --------------- |
| Remove untracked files              | `git clean -f`  |
| Remove untracked files/dirs         | `git clean -fd` |
| Dry run (see what would be deleted) | `git clean -n`  |

Reach out: https://guns.lol/january1073
