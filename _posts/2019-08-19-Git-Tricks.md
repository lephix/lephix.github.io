# Git tricks

## Situation: Local changes is not committed.

Use `git pull --rebase` instead of `git pull`. This action will rebase the current branch base on the remote branch.

## Situation: When the there is a local commit base on the stale remote branch, and the commit is not pushed yet.

# reset the local commit.
git reset ${commit_id}

# rebase base on the remote branch.
git fetch
git rebase origin/master

# fix the conflict
git add .
git commit -m "new comments".
git push
