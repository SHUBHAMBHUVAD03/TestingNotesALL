# 🗂️ Git & GitHub Complete Command Reference
# Use this file in VS Code Terminal (Ctrl + ` to open terminal)
# ============================================================

# ──────────────────────────────────────────────────────────────
# 1. GIT SETUP & CONFIGURATION
# ──────────────────────────────────────────────────────────────

git --version
#### Check installed Git version

git config --global user.name "Your Name"
#### Set your global username for all commits

git config --global user.email "you@example.com"
#### Set your global email for all commits

git config --global core.editor "code --wait"
#### Set VS Code as the default Git editor

git config --list
#### List all Git configuration settings

git config --global color.ui auto
#### Enable colored output in terminal

git config --global alias.st status
#### Create a shortcut alias: 'git st' instead of 'git status'

git config --global alias.co checkout
#### Create alias: 'git co' instead of 'git checkout'

git config --global alias.br branch
#### Create alias: 'git br' instead of 'git branch'

git config --global alias.lg "log --oneline --graph --all"
#### Create alias for a pretty log view


# ──────────────────────────────────────────────────────────────
# 2. INITIALIZING A REPOSITORY
# ──────────────────────────────────────────────────────────────

git init
#### Initialize a new local Git repository in the current folder

git init my-project
#### Initialize a new Git repository inside a folder named 'my-project'

git clone https://github.com/user/repo.git
#### Clone a remote repository to your local machine

git clone https://github.com/user/repo.git my-folder
#### Clone into a specific local folder name

git clone --depth 1 https://github.com/user/repo.git
#### Shallow clone — only download the latest snapshot (faster)


# ──────────────────────────────────────────────────────────────
# 3. BASIC SNAPSHOTTING (STAGE & COMMIT)
# ──────────────────────────────────────────────────────────────

git status
#### Show working directory status — staged, unstaged, untracked files

git add filename.txt
#### Stage a specific file for commit

git add .
#### Stage ALL changed and new files in the current directory

git add -A
#### Stage ALL changes including deletions

git add -p
#### Interactively stage parts (hunks) of files

git commit -m "Your commit message"
#### Commit staged changes with a message

git commit -am "Your message"
#### Stage all tracked modified files AND commit in one step

git commit --amend -m "Updated message"
#### Modify the last commit message (only if not pushed yet)

git commit --amend --no-edit
#### Add staged changes to the last commit without changing the message

git reset HEAD filename.txt
#### Unstage a file (keep changes in working directory)

git restore filename.txt
#### Discard changes in working directory (revert to last commit)

git restore --staged filename.txt
#### Unstage a file without discarding changes

git rm filename.txt
#### Remove a file from working directory AND stage the deletion

git rm --cached filename.txt
#### Remove a file from tracking (keep it locally)

git mv oldname.txt newname.txt
#### Rename or move a file and stage the change


# ──────────────────────────────────────────────────────────────
# 4. VIEWING HISTORY & DIFFERENCES
# ──────────────────────────────────────────────────────────────

git log
#### Show full commit history

git log --oneline
#### Show compact one-line commit history

git log --oneline --graph --all
#### Show a visual branch graph of all commits

git log --author="Name"
#### Filter commits by author name

git log --since="2024-01-01" --until="2024-12-31"
#### Show commits within a date range

git log -n 5
#### Show the last 5 commits

git log --follow filename.txt
#### Show history of a specific file (even across renames)

git log --stat
#### Show commit history with file change stats

git log -p
#### Show commit history with full diffs

git show abc1234
#### Show details and diff of a specific commit

git diff
#### Show unstaged changes (working dir vs last commit)

git diff --staged
#### Show staged changes (staged vs last commit)

git diff main feature-branch
#### Compare two branches

git diff abc1234 def5678
#### Compare two specific commits

git blame filename.txt
#### Show who changed each line of a file and when

git shortlog -sn
#### Summary of commits per author


# ──────────────────────────────────────────────────────────────
# 5. BRANCHING
# ──────────────────────────────────────────────────────────────

git branch
#### List all local branches

git branch -a
#### List all branches (local + remote)

git branch -r
#### List only remote branches

git branch feature-login
#### Create a new branch named 'feature-login'

git checkout feature-login
#### Switch to 'feature-login' branch

git checkout -b feature-login
#### Create AND switch to a new branch in one command

git switch feature-login
#### Switch to a branch (modern syntax)

git switch -c feature-login
#### Create AND switch to a branch (modern syntax)

git branch -d feature-login
#### Delete a merged branch (safe delete)

git branch -D feature-login
#### Force delete a branch (even if unmerged)

git branch -m old-name new-name
#### List branches already merged into current branch

git branch --no-merged
#### List branches NOT yet merged


# ──────────────────────────────────────────────────────────────
# 6. MERGING
# ──────────────────────────────────────────────────────────────

git merge feature-login
#### Merge 'feature-login' branch into the current branch

git merge --no-ff feature-login
#### Merge with a merge commit (no fast-forward)

git merge --squash feature-login
#### Squash all commits from branch into one before merging

git merge --abort
#### Abort an in-progress merge with conflicts

git mergetool
#### Open a visual merge tool to resolve conflicts


# ──────────────────────────────────────────────────────────────
# 7. REBASING
# ──────────────────────────────────────────────────────────────

git rebase main
#### Rebase current branch onto main (replay commits)

git rebase -i HEAD~3
#### Interactive rebase — edit, squash, or reorder last 3 commits

git rebase --continue
#### Continue rebase after resolving conflicts

git rebase --skip
#### Skip the current conflicting commit during rebase

git rebase --abort
#### Cancel the rebase and return to original state


# ──────────────────────────────────────────────────────────────
# 8. REMOTE REPOSITORIES
# ──────────────────────────────────────────────────────────────

git remote -v
#### List all configured remote connections

git remote add origin https://github.com/user/repo.git
#### Add a remote named 'origin'

git remote rename origin upstream
#### Rename a remote connection

git remote remove origin
#### Remove a remote connection

git remote set-url origin https://github.com/user/new-repo.git
#### Change the URL of an existing remote

git fetch origin
#### Download all changes from remote (without merging)

git fetch --all
#### Fetch from all configured remotes

git pull origin main
#### Fetch AND merge from remote 'main' branch

git pull --rebase origin main
#### Fetch AND rebase instead of merge

git push origin main
#### Push local 'main' branch to remote 'origin'

git push -u origin feature-login
#### Push branch and set it to track the remote branch

git push --force
#### Force push (overwrites remote history — use carefully!)

git push --force-with-lease
#### Safe force push (fails if remote has new commits)

git push origin --delete feature-login
#### Delete a remote branch

git push --tags
#### Push all local tags to remote


# ──────────────────────────────────────────────────────────────
# 9. STASHING
# ──────────────────────────────────────────────────────────────

git stash
#### Temporarily save uncommitted changes to a stack

git stash push -m "work in progress"
#### Stash with a descriptive message

git stash list
#### List all stashed entries

git stash pop
#### Apply the most recent stash and remove it from stack

git stash apply stash@{1}
#### Apply a specific stash (keep it in stack)

git stash drop stash@{0}
#### Delete a specific stash entry

git stash clear
#### Delete ALL stash entries

git stash branch feature-temp
#### Create a new branch from the stash


# ──────────────────────────────────────────────────────────────
# 10. UNDOING CHANGES
# ──────────────────────────────────────────────────────────────

git revert abc1234
#### Create a new commit that undoes a specific commit (safe)

git reset --soft HEAD~1
#### Undo last commit, keep changes staged

git reset --mixed HEAD~1
#### Undo last commit, keep changes unstaged (default)

git reset --hard HEAD~1
#### Undo last commit and DISCARD all changes (destructive!)

git reset --hard origin/main
#### Reset local branch to match remote exactly

git clean -fd
#### Delete all untracked files and directories

git clean -n
#### Dry run — show what would be deleted by clean

git reflog
#### Show a log of all HEAD movements (great for recovery!)


# ──────────────────────────────────────────────────────────────
# 11. TAGGING
# ──────────────────────────────────────────────────────────────

git tag
#### List all existing tags

git tag v1.0.0
#### Create a lightweight tag at the current commit

git tag -a v1.0.0 -m "Release version 1.0.0"
#### Create an annotated tag with a message

git tag -a v1.0.0 abc1234
#### Tag a specific past commit

git show v1.0.0
#### Show details of a tag

git push origin v1.0.0
#### Push a single tag to remote

git push origin --tags
#### Push all tags to remote

git tag -d v1.0.0
#### Delete a local tag

git push origin --delete v1.0.0
#### Delete a remote tag


# ──────────────────────────────────────────────────────────────
# 12. GIT IGNORE
# ──────────────────────────────────────────────────────────────

# Create a .gitignore file:
# In VS Code terminal run:
touch .gitignore
# Then add patterns to ignore, e.g.:
#   node_modules/
#   *.log
#   .env
#   target/
#   *.class

git check-ignore -v filename.txt
# Debug: check if and why a file is ignored

git rm -r --cached .
# Clear cache after updating .gitignore (then re-add and commit)


# ──────────────────────────────────────────────────────────────
# 13. CHERRY-PICK
# ──────────────────────────────────────────────────────────────

git cherry-pick abc1234
# Apply a specific commit from another branch to current branch

git cherry-pick abc1234 def5678
# Apply multiple specific commits

git cherry-pick --no-commit abc1234
# Apply commit changes without creating a commit

git cherry-pick --abort
# Abort a cherry-pick in progress


# ──────────────────────────────────────────────────────────────
# 14. SUBMODULES
# ──────────────────────────────────────────────────────────────

git submodule add https://github.com/user/lib.git libs/lib
# Add an external repo as a submodule

git submodule init
# Initialize submodule configuration

git submodule update
# Pull the latest content for all submodules

git submodule update --remote
# Fetch and update submodules to latest remote commit

git submodule foreach git pull origin main
# Pull latest changes in all submodules


# ──────────────────────────────────────────────────────────────
# 15. SEARCHING
# ──────────────────────────────────────────────────────────────

git grep "TODO"
# Search for a string in the working directory

git grep "searchText" v1.0.0
# Search for a string in a specific tag/commit

git log -S "functionName"
# Find commits that added or removed a specific string

git log --all --full-history -- "**/filename.txt"
# Find all commits that touched a specific file


# ──────────────────────────────────────────────────────────────
# 16. GITHUB CLI (gh) COMMANDS
# Install: https://cli.github.com
# ──────────────────────────────────────────────────────────────

gh auth login
# Authenticate with your GitHub account

gh auth status
# Check current authentication status

gh auth logout
# Log out from GitHub CLI


# ── REPOSITORIES ──────────────────────────────────────────────

gh repo create my-repo --public
# Create a new public GitHub repository

gh repo create my-repo --private
# Create a new private GitHub repository

gh repo clone user/repo
# Clone a GitHub repository

gh repo view
# View the current repository in browser

gh repo list
# List your GitHub repositories

gh repo delete user/repo
# Delete a GitHub repository (with confirmation)

gh repo fork user/repo
# Fork a GitHub repository to your account


# ── PULL REQUESTS ─────────────────────────────────────────────

gh pr create
# Create a pull request interactively

gh pr create --title "Fix login bug" --body "Fixes #42" --base main
# Create a PR with title, body, and target branch

gh pr list
# List open pull requests

gh pr view 15
# View details of PR #15

gh pr checkout 15
# Check out a PR branch locally

gh pr merge 15 --squash
# Merge PR #15 using squash merge

gh pr merge 15 --rebase
# Merge PR #15 using rebase

gh pr close 15
# Close a pull request without merging

gh pr review 15 --approve
# Approve a pull request

gh pr review 15 --request-changes --body "Please fix the tests"
# Request changes on a PR

gh pr diff 15
# View the diff of a pull request

gh pr status
# Show PRs relevant to you (created, review requested, etc.)


# ── ISSUES ────────────────────────────────────────────────────

gh issue create --title "Bug: crash on login" --body "Steps to reproduce..."
# Create a new issue

gh issue list
# List open issues

gh issue view 10
# View issue #10

gh issue close 10
# Close issue #10

gh issue reopen 10
# Reopen a closed issue

gh issue comment 10 --body "Will fix in next release"
# Add a comment to an issue

gh issue assign 10 --assignee username
# Assign an issue to a user


# ── WORKFLOWS & GITHUB ACTIONS ────────────────────────────────

gh workflow list
# List all GitHub Actions workflows

gh workflow run deploy.yml
# Manually trigger a workflow

gh run list
# List recent workflow runs

gh run view 123456
# View details of a specific run

gh run watch 123456
# Watch a live workflow run

gh run download 123456
# Download artifacts from a run


# ── GISTS ─────────────────────────────────────────────────────

gh gist create filename.txt
# Create a public gist from a file

gh gist create filename.txt --secret
# Create a private (secret) gist

gh gist list
# List your gists

gh gist view abc123
# View a gist by ID


# ── RELEASES ──────────────────────────────────────────────────

gh release create v1.0.0 --title "Version 1.0.0" --notes "Initial release"
# Create a new GitHub release

gh release list
# List all releases

gh release view v1.0.0
# View a specific release

gh release download v1.0.0
# Download release assets

gh release delete v1.0.0
# Delete a release


# ── SSH KEYS ──────────────────────────────────────────────────

gh ssh-key list
# List SSH keys on your GitHub account

gh ssh-key add ~/.ssh/id_rsa.pub --title "My Laptop"
# Add a new SSH key to GitHub


# ──────────────────────────────────────────────────────────────
# 17. USEFUL VS CODE TERMINAL TIPS
# ──────────────────────────────────────────────────────────────

# Open VS Code terminal: Ctrl + ` (backtick)
# Split terminal: Ctrl + Shift + 5
# New terminal: Ctrl + Shift + `

# Open current repo in VS Code:
code .

# Open a specific file in VS Code:
code filename.txt

# Run git log with pretty graph (paste this):
git log --oneline --graph --decorate --all

# Set VS Code as default merge tool:
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Set VS Code as default diff tool:
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

# Open merge conflicts in VS Code:
git mergetool

# Open diff in VS Code:
git difftool


# ──────────────────────────────────────────────────────────────
# 18. COMMON WORKFLOWS (QUICK REFERENCE)
# ──────────────────────────────────────────────────────────────

# --- Start a new feature ---
# git checkout -b feature/my-feature
# git add .
# git commit -m "Add my feature"
# git push -u origin feature/my-feature
# (then create a PR on GitHub)

# --- Sync fork with upstream ---
# git remote add upstream https://github.com/original/repo.git
# git fetch upstream
# git checkout main
# git merge upstream/main
# git push origin main

# --- Undo last push (use with caution!) ---
# git reset --hard HEAD~1
# git push --force-with-lease

# --- Squash last N commits into one ---
# git rebase -i HEAD~N
# (change 'pick' to 'squash' for the commits to squash)

# --- Save credentials permanently ---
# git config --global credential.helper store

# --- See size of repo ---
# git count-objects -vH

# ============================================================
# END OF GIT & GITHUB COMMAND REFERENCE
# ============================================================
