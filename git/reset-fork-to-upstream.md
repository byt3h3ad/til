# Quick and Dirty Way to Reset Your Fork to Match Upstream

I forked a repo, make a PR, and it got merged, but I did the changes in my fork’s `main`. It now showed “X commits ahead, Y behind”.

Those “ahead” commits are already in upstream, but still duplicated in my fork.

### Fix

First of all, never make the same mistake as me and commit directly in the `main` branch. Always make separate branches.

```bash
# Make sure you have upstream set
git remote add upstream https://github.com/ORIGINAL_OWNER/REPO.git

# Fetch latest upstream changes
git fetch upstream

# Checkout your default branch
git checkout main   # or master, depending on repo

# Reset your branch to match upstream exactly
git reset --hard upstream/main

# Push the clean state to your fork
git push origin main --force
```

Or if you're lazy like me and like to use [zsh git plugin](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git):

```bash
# Assumes:
# - upstream remote is set (gurl upstream to check)
# - oh-my-zsh git plugin enabled

gfa                         # git fetch --all (includes upstream)
gco main                    # git checkout main
grhh upstream/main          # git reset --hard upstream/main
ggf                         # git push origin main --force
```
