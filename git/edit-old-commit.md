# Editing Old Commits Using CLI or with Lazygit

Whenever I needed to edit an old commit, be it a silly typo or a missing import I caught before pushing to a remote repository, I use the following `git` commands.

```git
git log # look up the commit hash to be edited
git rebase --interactive 'abcdef^' # caret means its parent
# mark the commit as "edit" and apply changes
git commit --all --amend --no-edit
git rebase --continue
```

Using [lazygit](https://github.com/jesseduffield/lazygit), the process is almost identical.

- Navigate to the commit to be edited and press `e` _(edit commit)_.
- Stage changes and press `A` _(Amend commit with staged changes)_.
- Press `m` _(view merge/rebase options)_ and select `continue`.
