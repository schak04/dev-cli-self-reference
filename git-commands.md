# Git Commands CheatSheet

## Setup & Configuration

- `git config --global user.name "Your Name"` - Set Git username
- `git config --global user.email "you@example.com"` - Set Git email
- `git config --global core.editor code` - Set VS Code as default editor
- `git config --global core.editor "vim"` - Set Vim as default editor
- `git config --list` - View all config settings

## Creating & Cloning Repositories

- `git init` - Initialize new Git repository
- `git clone <url>` - Clone an existing repo

## Staging & Committing

- `git add <file>` - Stage a specific file
- `git add .` - Stage all files
- `git commit -m "message"` - Commit staged changes (I'll create a separate file for commit message best practices)
- `git status` - Show current status

## Branching & Merging

- `git branch` - List all branches
- `git branch <branch-name>` - Create a new branch
- `git checkout <branch>` or `git switch <branch>` - Switch to an existing branch
- `git checkout -b <name>` or `git switch -c <name>` - Create and switch to a new branch
- `git merge <branch-name>` - Merge a branch into the current branch
- `git merge --abort` - Abort a merge (e.g., on conflict)

## Remote Repositories

- `git remote add origin <url>` - Link remote repo as 'origin'
- `git remote -v` - View remotes
- `git push -u origin <branch>` - Push and set upstream tracking (after using `-u` once, you can just type `git push` from the next time)
- `git pull` - Fetch and merge from remote

## Undo & Reset

- `git restore <file>` - Discard changes in a file
- `git restore --staged <file>` - Unstage a file, keep changes
- `git reset <file>` - Unstage specific file
- `git reset --soft HEAD~1` - Undo last commit, keep staged
- `git reset --mixed HEAD~1` - Undo commit, unstage changes
- `git reset --hard HEAD~1` - **CAUTION:** Permanently discards the commit and local changes
- `git reset --hard HEAD~N` - **CAUTION:** Like above, but resets back **N commits** and discards local changes
- `git reset --hard <commit_hash>` - **CAUTION:** This discards all changes after that commit
- `git checkout -- <file>` - Discard changes in a file

## Viewing History

- `git log` - Full commit log
- `git log --oneline` - Compact view
- `git show <commit>` - View specific commit
- `git diff` - Show unstaged changes
- `git diff --staged` - Show staged changes
- `git blame <file>` - See line-by-line changes with authors

## Stashing & Cleaning

- `git stash` - Save current changes and revert to the last commit
- `git stash pop` - Reapply the last stash and remove it from the stash list
- `git stash apply` - Reapply the last stash but keep it in the stash list
- `git stash list` - List all stashes
- `git stash drop` - Remove last stash
- `git stash clear` - **CAUTION:** Deletes all stashes
- `git clean -fd` - **CAUTION:** Deletes **untracked** files and directories irreversibly
- `git clean -nfd` - Dry-run (see what would be deleted, without deleting)

## Remove Tracked Files (Now in .gitignore)

- `git rm --cached <file-name>` - Untrack a specific file
- `git rm -r --cached <folder-name>` - Untrack a specific folder
- `git rm -r --cached .` - Untrack **all files**, including those now in `.gitignore`
    > Use this carefully and make sure you don't untrack files you still want in Git.
- `git add .` - Re-add tracked files
- `git commit -m "Clean up ignored files"` or whatever commit message fits
- `git push` - Push cleanup

## Edit Commits

- `git commit --amend` - Edit last commit message
- `git push --force` - Required after amending
- `git rebase -i HEAD~N` - Edit N commits back:
  - Opens the default editor
  - Change `pick` to `reword` in editor
  - Save and update messages
  - Then `git push --force`
  > Rewrites history. Safe for solo branches or repos. Risky on shared ones; coordinate with teammates before use.

## About Force-Pushing

Force-pushing overwrites the commit history on the remote repository. It is a powerful tool that should be used with caution, especially in shared repositories.  
While **force-pushing is safe if you're the only developer**, it's still good practice to use it thoughtfully.

### Best practices

- Prefer using `git push --force-with-lease` instead of `git push --force`, as it ensures you don't accidentally overwrite someone else's work.
- Avoid force-pushing to shared branches unless absolutely necessary and after coordinating with your team.

## Disaster Recovery

- `git reflog` - Show history of HEAD changes
- `git reset --hard <reflog_hash>` - Restores old state; overwrites current work
- `git rev-list --max-parents=0 HEAD` - Get the first commit hash
- `git checkout --orphan fresh-start` - Start a new history
- `git branch -D <branch>` - Delete a branch
- `git branch -m <new-name>` - Rename current branch

## Advanced Commands

- `git cherry-pick <commit>` - Apply a commit from elsewhere
- `git bisect` - Binary search to find a bad commit
- `git tag <tagname>` - Create a tag
- `git tag -a v1.0 -m "msg"` - Create annotated tag
- `git push origin <tagname>` - Push a specific tag
- `git apply <patchfile>` - Apply a patch

## Pull Requests & Fork Merges

- `git push origin feature-branch` - Push branch
- Using GitHub CLI:
  - `gh auth login` - Log in
  - `gh pr create --base main --head feature-branch --title "PR title" --body "Changes description"`
- Manual merge from fork:
  - `git remote add theirname https://github.com/theirusername/their-fork.git`
  - `git fetch theirname feature/branch`
  - `git checkout main`
  - `git merge theirname/feature/branch`
  - `git remote remove theirname`

## Git Tags & Releases

- `git tag` - List tags
- `git tag -a v1.0.0 -m "Message"` - Create annotated tag
- `git tag -d v1.0.0` - Delete local tag
- `git push origin v1.0.0` - Push single tag
- `git push origin --tags` - Push all tags
- `git show v1.0.0` - Show tag info
- `git push origin :refs/tags/v1.0.0` - Delete remote tag

### Creating GitHub Release

- Push the tag
- Go to GitHub → Releases → Draft new release
- Choose tag, title, and description → Publish

### Semantic Versioning

- Format: MAJOR.MINOR.PATCH (e.g., 1.2.3)
  - MAJOR: Breaking changes
  - MINOR: New features (backward compatible)
  - PATCH: Bug fixes
- Examples:
  - First stable release: `v1.0.0`
  - New feature: `v1.1.0`
  - Fix: `v1.1.1`
  - Breaking change: `v2.0.0`

## Git LFS (Large File Storage)

Git LFS is an extension for Git that manages large files by storing them outside your main Git repository.

### Installation

**CLI:**

```bash
# Ubuntu/Debian
sudo apt install git-lfs

# Windows (via Chocolatey)
choco install git-lfs

# macOS
brew install git-lfs
```

**GUI:**

Download and install from [https://git-lfs.com/](https://git-lfs.com/)

### Initialize Git LFS

Run once per system/user (no matter in which directory, it will always get installed globally):

```bash
git lfs install
```

### Track File Types

Run before adding/committing large files:

```bash
git lfs track file/files
```

This updates the `.gitattributes` file. Don't forget to commit it:

```bash
git add .gitattributes
git commit -m "Track large files with Git LFS"
```

### Clone or Pull with LFS

`git clone <repo-url>` - Automatically fetches LFS files
`git pull` - Pulls LFS files if tracked

### Push LFS-Tracked Files

```bash
git add largefile.extension
git commit -m "Add largefile.extension with LFS"
git push
```

### View Tracked File Types

```bash
git lfs track
```

### Untrack File Types

```bash
git lfs untrack file/files
git add .gitattributes
git commit -m "Stop tracking XYZ files with Git LFS"
```

### Remove LFS Files from History (Advanced)

To clean LFS files from Git history, use tools like:

- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- `git filter-branch`

> **Note:** These operations rewrite history. Use with caution.

---
