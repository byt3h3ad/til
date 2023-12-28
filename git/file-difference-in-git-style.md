# Show file difference in `git` style

```bash
git diff --no-index <path1> <path2>
```

Normally, `git diff` compares the working tree or staging area to a specific commit or another commit. With `--no-index`, you can compare any two paths on your filesystem, regardless of their inclusion in the Git repository. Useful for comparing changes before staging them or for comparing working copies of files outside the repository. Offers similar functionality to the diff command but integrates with Git features like highlighting staged changes.

You can also use wildcards, like `git diff --no-index src/*.js my_changes/*.js` to compare all files with the `.js` extension in both directories.

[source](https://git-scm.com/docs/git-diff#Documentation/git-diff.txt-emgitdiffemltoptionsgt--no-index--ltpathgtltpathgt)

