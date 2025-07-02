# Git Commands CheatSheet

---

## Setup & Configuration

- `git config --global user.name "Your Name"` - Set Git username
- `git config --global user.email "you@example.com"` - Set Git email
- `git config --global core.editor code` - Set VS Code as default editor  
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
- `git reset --hard HEAD~1` - Undo commit and changes
- `git reset --hard HEAD~5` - Reset back 5 commits
- `git reset --hard <commit_hash>` - Reset to a specific commit
- `git checkout -- <file>` - Discard changes in a file

## Viewing History

- `git log` - Full commit log
- `git log --oneline` - Compact view
- `git show <commit>` - View specific commit
- `git diff` - Show unstaged changes
- `git diff --staged` - Show staged changes
- `git blame <file>` - See line-by-line changes with authors

## Stashing & Cleaning

- `git stash` - Save current changes
- `git stash pop` - Reapply last stash
- `git stash list` - List all stashes
- `git stash drop` - Remove last stash
- `git clean -fd` - Delete untracked files and directories

## Remove Tracked Files (Now in .gitignore)

- `git rm -r --cached .` - Untrack all ignored files
- `git add .` - Re-add tracked files
- `git commit -m "Clean up ignored files"`
- `git push` - Push cleanup

## Edit Commits

- `git commit --amend` - Edit last commit message
- `git push --force` - Required after amending
- `git rebase -i HEAD~N` - Edit N commits back:
  - Change `pick` to `reword` in editor
  - Save and update messages
  - Then `git push --force`

## Reset Entire Git History

- `rm -rf .git` - Delete entire Git history
- Unlink, delete, and recreate remote if needed (for a clean commit & activity history), and then:
  - `git init`
  - `git add .`
  - `git commit -m "Initial commit"`
  - `git remote add origin <url>`
  - `git push -u origin main` or `--force` if remote already exists (it'd need force-pushing if it wasn't unlinked, deleted and recreated)

## Disaster Recovery

- `git reflog` - Show history of HEAD changes
- `git reset --hard <reflog_hash>` - Restore to a previous HEAD state
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

### About Force-Pushing

Force-pushing overwrites commit history on the remote repository. This can permanently erase commits and disrupt the ability to recover previous work. It is a powerful tool, but should be used with caution.

**Best practices:**

- Avoid force-pushing to important branches like `main` or `master` unless absolutely necessary.
- Prefer using `git push --force-with-lease` instead of `git push --force` to prevent overwriting others' work by mistake.
- Always double-check which branch you are on before force-pushing.
- Force-pushing is safe if you are the only developer, but always be aware of the risks and use it thoughtfully.

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
