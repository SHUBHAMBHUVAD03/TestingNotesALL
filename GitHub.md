# 🐙 Git & GitHub Command Reference

> 💡 Open terminal in VS Code → **Ctrl + `**

## Contents
- [Setup & Config](#setup--config)
- [Initialize Repo](#initialize-repo)
- [Stage & Commit](#stage--commit)
- [History & Diffs](#history--diffs)
- [Branching](#branching)
- [Merging](#merging)
- [Rebasing](#rebasing)
- [Remote Repos](#remote-repos)
- [Stashing](#stashing)
- [Undoing Changes](#undoing-changes)
- [Tagging](#tagging)
- [Git Ignore](#git-ignore)
- [Cherry-Pick](#cherry-pick)
- [Searching](#searching)
- [GitHub CLI](#github-cli)
- [VS Code Tips](#vs-code-tips)
- [Common Workflows](#common-workflows)

## Setup & Config

```bash
git --version                                        # Check Git version
git config --global user.name "Your Name"            # Set username
git config --global user.email "you@example.com"     # Set email
git config --global core.editor "code --wait"        # Set VS Code as editor
git config --list                                    # View all configs
git config --global color.ui auto                    # Enable colored output
```

#### Aliases

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all"
```

## Initialize Repo

```bash
git init                                             # Init repo in current folder
git init my-project                                  # Init repo in new folder
git clone https://github.com/user/repo.git           # Clone a repo
git clone https://github.com/user/repo.git my-folder # Clone into specific folder
git clone --depth 1 https://github.com/user/repo.git # Shallow clone (faster)
```

## Stage & Commit

#### Staging

```bash
git status                          # Show changed, staged, untracked files
git add filename.txt                # Stage a specific file
git add .                           # Stage all changes
git add -A                          # Stage all including deletions
git add -p                          # Interactively stage parts of a file
git restore filename.txt            # Discard changes in working directory
git restore --staged filename.txt   # Unstage a file (keep changes)
git rm filename.txt                 # Remove file and stage deletion
git rm --cached filename.txt        # Stop tracking file (keep on disk)
git mv old.txt new.txt              # Rename or move a file
```

#### Committing

```bash
git commit -m "message"             # Commit staged changes
git commit -am "message"            # Stage tracked files + commit in one step
git commit --amend -m "new message" # Edit last commit message
git commit --amend --no-edit        # Add staged changes to last commit
```

## History & Diffs

#### Log

```bash
git log                             # Full commit history
git log --oneline                   # Compact one-line format
git log --oneline --graph --all     # Visual branch graph
git log --author="Name"             # Filter by author
git log --since="2024-01-01"        # Filter by date
git log -n 5                        # Last 5 commits
git log --stat                      # Files changed per commit
git log -p                          # Full diff per commit
git log --follow filename.txt       # History of a specific file
git show abc1234                    # Show a specific commit
git shortlog -sn                    # Commit count per author
```

#### Diff

```bash
git diff                            # Unstaged changes
git diff --staged                   # Staged changes
git diff main feature-branch        # Compare two branches
git diff abc1234 def5678            # Compare two commits
git blame filename.txt              # Who changed each line and when
```

## Branching

```bash
git branch                          # List local branches
git branch -a                       # List all branches (local + remote)
git branch -r                       # List remote branches
git branch feature-login            # Create a new branch
git switch feature-login            # Switch to a branch
git switch -c feature-login         # Create AND switch (use this most)
git checkout -b feature-login       # Old style (still works)
git branch -m old-name new-name     # Rename a branch
git branch -d feature-login         # Delete merged branch (safe)
git branch -D feature-login         # Force delete branch
git branch --merged                 # List merged branches
git branch --no-merged              # List unmerged branches
```

> 💡 Prefer `git switch` over `git checkout` — it's cleaner and newer.

## Merging

```bash
git merge feature-login             # Merge into current branch
git merge --no-ff feature-login     # Merge with a merge commit
git merge --squash feature-login    # Squash all commits into one
git merge --abort                   # Cancel a conflicted merge
git mergetool                       # Open visual conflict resolver
```

## Rebasing

> ⚠️ Never rebase commits already pushed to a shared branch.

```bash
git rebase main                     # Rebase onto main
git rebase -i HEAD~3                # Interactively edit last 3 commits
git rebase --continue               # Continue after fixing conflict
git rebase --skip                   # Skip the conflicting commit
git rebase --abort                  # Cancel and restore original state
```

## Remote Repos

#### Manage Connections

```bash
git remote -v                                        # List remotes
git remote add origin https://github.com/user/repo   # Add a remote
git remote set-url origin https://github.com/new     # Change remote URL
git remote rename origin upstream                    # Rename a remote
git remote remove origin                             # Remove a remote
```

#### Fetch, Pull & Push

```bash
git fetch origin                    # Download changes (no merge)
git fetch --all                     # Fetch from all remotes
git pull origin main                # Fetch + merge
git pull --rebase origin main       # Fetch + rebase
git push origin main                # Push to remote
git push -u origin feature-login    # Push + set tracking
git push --tags                     # Push all tags
git push origin --delete feature    # Delete remote branch
git push --force-with-lease         # Safe force push ✅
git push --force                    # Dangerous force push ❌
```

## Stashing

```bash
git stash                           # Save changes to stack
git stash push -m "WIP: login"      # Stash with a label
git stash list                      # List all stashes
git stash pop                       # Apply latest + remove from stack
git stash apply stash@{1}           # Apply specific stash (keep in list)
git stash drop stash@{0}            # Delete a specific stash
git stash clear                     # Delete ALL stashes
git stash branch new-branch         # Create a branch from stash
```

## Undoing Changes

#### Safe Undo

```bash
git revert abc1234                  # New commit that reverses a past commit
```

#### Reset (rewrites history)

```bash
git reset --soft HEAD~1             # Undo commit — keep changes staged
git reset --mixed HEAD~1            # Undo commit — keep changes unstaged
git reset --hard HEAD~1             # Undo commit — delete all changes ⚠️
git reset --hard origin/main        # Reset branch to match remote ⚠️
```

#### Clean & Recover

```bash
git clean -n                        # Preview what would be deleted
git clean -fd                       # Delete all untracked files
git reflog                          # See all HEAD movements (recovery log)
```

> 💡 `git reflog` can save you even after a bad `reset --hard`.

## Tagging

```bash
git tag                             # List all tags
git tag v1.0.0                      # Create lightweight tag
git tag -a v1.0.0 -m "Release"      # Create annotated tag
git tag -a v1.0.0 abc1234 -m "Fix"  # Tag a past commit
git show v1.0.0                     # Show tag details
git push origin v1.0.0              # Push one tag
git push origin --tags              # Push all tags
git tag -d v1.0.0                   # Delete local tag
git push origin --delete v1.0.0     # Delete remote tag
```

## Git Ignore

```bash
touch .gitignore                    # Create the file
git check-ignore -v filename.txt    # Debug why a file is ignored
git rm -r --cached .                # Clear cache after updating .gitignore
```

#### Common `.gitignore` Patterns

```
node_modules/
target/
*.class
*.jar
.env
.env.local
.DS_Store
Thumbs.db
*.log
build/
dist/
out/
```

> After `git rm -r --cached .` → run `git add .` then `git commit -m "update gitignore"`

## Cherry-Pick

```bash
git cherry-pick abc1234             # Apply a specific commit
git cherry-pick abc1234 def5678     # Apply multiple commits
git cherry-pick --no-commit abc1234 # Apply changes without committing
git cherry-pick --abort             # Cancel cherry-pick in progress
```

## Searching

```bash
git grep "TODO"                                      # Search text in project
git grep "text" v1.0.0                               # Search in a tag/commit
git log -S "functionName"                            # Find commits by code change
git log --all --full-history -- "**/MyFile.java"     # All commits touching a file
```

## GitHub CLI

> Install: **https://cli.github.com**

#### Auth

```bash
gh auth login                       # Login to GitHub
gh auth status                      # Check login status
gh auth logout                      # Logout
```

#### Repos

```bash
gh repo create my-repo --public     # Create public repo
gh repo create my-repo --private    # Create private repo
gh repo clone user/repo             # Clone a repo
gh repo view                        # Open repo in browser
gh repo list                        # List your repos
gh repo fork user/repo              # Fork a repo
gh repo delete user/repo            # Delete a repo
```

#### Pull Requests

```bash
gh pr create                                                    # Create PR interactively
gh pr create --title "Fix" --body "Closes #42" --base main      # Create with details
gh pr list                                                      # List open PRs
gh pr view 15                                                   # View PR #15
gh pr checkout 15                                               # Checkout PR locally
gh pr merge 15 --squash                                         # Merge with squash
gh pr merge 15 --rebase                                         # Merge with rebase
gh pr close 15                                                  # Close without merging
gh pr review 15 --approve                                       # Approve a PR
gh pr review 15 --request-changes --body "Add tests"            # Request changes
gh pr status                                                    # Your PRs overview
```

#### Issues

```bash
gh issue create --title "Bug" --body "Details"   # Create an issue
gh issue list                                    # List open issues
gh issue view 10                                 # View issue #10
gh issue close 10                                # Close an issue
gh issue reopen 10                               # Reopen an issue
gh issue comment 10 --body "On it"               # Comment on issue
gh issue assign 10 --assignee username           # Assign an issue
```

#### GitHub Actions

```bash
gh workflow list                    # List all workflows
gh workflow run deploy.yml          # Trigger a workflow manually
gh run list                         # List recent runs
gh run view 123456                  # View a run
gh run watch 123456                 # Watch a live run
gh run download 123456              # Download run artifacts
```

#### Releases & Gists

```bash
gh release create v1.0.0 --title "v1.0.0" --notes "Release"    # Create release
gh release list                                                 # List releases
gh release download v1.0.0                                      # Download assets
gh gist create filename.txt                                     # Create public gist
gh gist create filename.txt --secret                            # Create private gist
gh gist list                                                    # List your gists
```

## VS Code Tips

```bash
code .                              # Open current folder in VS Code
code filename.txt                   # Open a specific file

# Set VS Code as merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Set VS Code as diff tool
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

git mergetool                       # Open conflicts in VS Code
git difftool                        # Open diff in VS Code
```

| Shortcut | Action |
|----------|--------|
| `Ctrl + `` ` | Open / close terminal |
| `Ctrl + Shift + `` ` | New terminal tab |
| `Ctrl + Shift + 5` | Split terminal |
| `Ctrl + Shift + V` | Markdown preview |

## Common Workflows

#### Start a New Feature

```bash
git switch main
git pull origin main
git switch -c feature/my-feature
git add .
git commit -m "feat: add my feature"
git push -u origin feature/my-feature
gh pr create --title "My Feature" --base main
```

#### Sync a Forked Repo

```bash
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

#### Undo Last Push

```bash
git reset --hard HEAD~1
git push --force-with-lease
```

#### Squash Last N Commits

```bash
git rebase -i HEAD~3
# Change 'pick' → 'squash' for commits to merge, then save
```

#### Tag and Release

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin v1.0.0
gh release create v1.0.0 --title "v1.0.0" --notes "Changelog here"
```

#### Recover a Deleted Branch

```bash
git reflog                          # Find the lost commit hash
git switch -c recovered-branch abc1234
```

#### Save Git Credentials

```bash
git config --global credential.helper store
```

> 📌 Preview this file in VS Code with **Ctrl + Shift + V**
