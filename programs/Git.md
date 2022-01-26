# GIT

## Configure
```bash
git config --global user.name "<NAME>"
git config --global user.email <EMAIL>
# git config --global core.editor gedit
git config --global core.editor "atom --wait"
# git config --global color.ui auto    # Colorize git output
# git config --unset core.editor # remove local config
```

## Initialize repository for a new project
```bash
git init
git init --bare   # For server-side repository (forbids checkouts), should be a folder named <name>.git
git clone --bare <existing_project> <name>.git   # Initialize bare repository from existing code
```

## Clone existing repository to work on it
```bash
git clone <url>            # Creates a new folder (same name as in the repo)
git clone <url> <folder>   # Choose a different folder
```

## Commits (points of change)
```bash
git status                       # Status of current change
git status -s                    # Condensed status
git log                          # Log of past changes
git log -p                       # Log with changed lines
git log --stat                   # Log with statistics
git add <file>                   # Stage a file
git add --all                    # Stage all modified files
git reset -- <file>              # Unstage file
git rm --cached <file>           # Stage the removal of a file (does not delete file from file system)
git commit -am 'Commit Message'  # Stage all modified files (but not the new ones!) and commit with a message
git commit -m 'Commit Message'   # Create a commit with a message
git commit --amend               # Fix last commit
git diff <commit1> <commit2>     # Compare two commits
git diff <commit>~ <commit>      # Compare commit with its parent (~ means "parent of")
git blame <file>                 # Show git commit annotations in each line of a file
git blame -L 40,60 <file>        # Limit analyzed lines
```

## Referencing commits
```bash
HEAD                                 # The most recent commit in the current branch
HEAD~ or HEAD~1 or HEAD^ or HEAD^1   # The commit's first parent
HEAD~2                               # The commit's first parent's first parent
HEAD^2                               # The commit's second parent (if from a merge)
<branch1>..<branch2>                 # Commits in <branch2> that are not in <branch1>
git rev-list <ref-or-range>          # Outputs just the commit hashes (for scripting)
```

## Branches

The main branch is called `master`.

```bash
git branch                                 # List local branches (-r = remote, -a = all)
git branch <branch>                        # Create branch
git branch -d <branch>                     # Delete branch
git branch <branch> -u <remote_branch>     # Change tracking reference
git branch -d <branch>                     # Delete branch
git checkout <branch>                      # Change current files to the branch version
git checkout <branch> -- <file>            # Checkout a single file only
git checkout -b <branch>                   # Create + checkout
git merge <branch>                         # Merge branch to the current one
git merge --strategy-option ours|theirs <branch>   # Merge while deciding who wins conflicts
git rebase <branch>                        # Apply commits of this branch on top of another (changes this one's history)
git rebase <branch1> <branch2>             # Apply commits of <branch2> branch on top of <branch1> (changes <branch2>'s history)
git rebase --onto master <branchA> <branchB>   # Apply commits between <branchA> (exclusive) and <branchB> (inclusive) onto master
git diff <branch1> <branch2> -- <FILE>     # Compare file between branches
```


## Remotes

The main remote is called `origin`.

```bash
git remote -v                 # List remotes
git remote add <name> <url>   # Add remote
git remote add --mirror=push <name> <url>     # Add remote as mirror (push is always --mirror)
git remote show <remote>      # List remote branches
git remote set-url <remote> <url>             # Change remote URL
git checkout -b <branch> <remote>/<branch>    # Import branch from remote (and track it, by default)
git fetch                     # Update current branch from remote
git fetch <remote> <branch>   # Update specific branch from remote
git fetch --all               # Fetch all tracking branches from remote
git pull                      # Fetch + merge
git pull <remote> <branch>    # Fetch + merge specific remote branch
git pull --rebase             # Fetch + rebase
git push                      # Send changes to remote
git push <remote> <branch>    # Send changes to specific branch to remote
git push <remote> -u <track>  # Send changes and track the remote branch
git push <remote> <mybranch>:<serverbranch>
git push --tags               # Send tags to remote
git push --mirror             # Send everything to remote
git push <remote> :<branch>   # Delete branch/tag in remote
```


## Tags
```bash
git tag                                        # List current tags
git tag -a v1.0 -m 'Initial Public Release'    # Create annotated tag
git tag v1.0                                   # Lightweight tag
git tag v1.0 <commit_hash>                     # Tag past commit
git tag -d v1.0                                # Delete tag
git push <remote> v1.0                         # Push tag to remote
git push <remote> --tags                       # Push all tags to remote
```

## Reset
```bash
git reset <commit>         # Reset state to <commit> but leave working tree untouched
git reset <commit> --soft  # Reset state to <commit> but leave working tree changes staged
git reset <commit> --hard  # Reset state and files to <commit>
git clean -fd              # Delete files and folders not tracked or ignored
git clean -fdx             # Delete files and folders not tracked (even those on .gitignore!)
```

## Worktrees

Worktrees are branches from the same repository checked out to a different file system folder.

```bash
git worktree list                       # List current worktrees
git worktree add ../<folder> <branch>   # Add new worktree
git worktree prune                      # Remove references to worktrees deleted from the file system
```


## Branch logs with graph and colors:
```bash
git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %<(80,trunc)%s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```

### Add to alias git graph:
```bash
git config --global alias.graph "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %<(80,trunc)%s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```


## .gitignore (list of ignored files and folders)
```bash
/file                   # Ignore file in root
file                    # Ignore file in any directory
**/file                 # Ignore file in any directory
folder/**               # Ignore everything in folder
!file                   # Don't ignore file
```

Local additions to .gitignore: `.git/info/exclude`


## Backup and restore

### Backup
```bash
git bundle create <projectname>.bundle --all      # Backup all branches
git bundle create <projectname>.bundle <branch>   # Backup a single branch
git clone <projectname>.bundle <projectname>      # Restore bundle
```

### Recover popped stash (or unreachable commit)
```bash
git fsck --no-reflogs          # List commits
git show <hash>                # To see info
git stash apply <hash>         # Apply stash
gitk --all $( git fsck --no-reflog | awk '/dangling commit/ {print $3}' )        # GUI log version
```

## Changing history

### Manually

```bash
git rebase -i --root      # Rebase interactive on the whole history
# Change to e the commits that you want to edit
# Save and close the editor
# Edit the files
git add --all
git commit -m 'The new commit message'
git rebase --continue
```

### Changing each message automatically

```bash
git filter-branch --msg-filter '<program that reads old message from stdin and writes new message to stdout>'
```

### Garbage collection
```bash
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```
