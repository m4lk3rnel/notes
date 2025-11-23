git/smartgit: keep practicing

`git reflog` - displays the `ref` history (understand what a `ref` is)
`git cherry-pick <commit-hash>` - copies a commit with the `commit-hash` to the currently checkout-ed branch
`git checkout <branch>` - used for moving to `branch`
`git branch` - displays all the local branches, if `-a` is provided, the command will include the remote branches.
`git merge` or `git rebase` - can help you 'lift up' the branch. Also useful if you have an empty branch and you want to keep it up-to-date with another branch. `git rebase` helps keeping a linear git history in contrast to `git merge`.
`HEAD` - points the current checked out commit or branch.

![[git-head-stackoverflow.png]]

`.git/HEAD` - the contents of this file specify what `HEAD` points to.
e.g. ![[githead.png]]

- `git log` - commit history
- `git branch` - list branches
- `git checkout <branch-name>` - switch to `<branch-name>`
- `git switch --detach <commit-hash>` (switch to commit without creating a new branch)
- `git switch -c <new-branch-name> <commit-hash>` (switch to commit and create a new branch from it)