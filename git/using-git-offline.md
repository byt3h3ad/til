# Using git offline

By design Git works well without any remote repository. Branching, staging and commiting files work just normally.

```bash
mkdir test
cd test
git init --initial-branch=main
git add -all
git commit -m "feat: init"
```

This works for a single machine but (obviously) won't work on multiple machines.

### Working with multiple machines using a USB drive

On one development machine mount the memory stick.

```bash
cd /path/to/memory/stick
mkdir repoName.git
cd repoName.git
git init --bare
```

Navigate to the repository that is to be shared, add the remote repository on the memory stick, and push the changes.

```bash
cd /path/to/local/repo/
git remote add origin /path/to/memory/stick/repoName.git
git push origin master
```

Unmount the memory stick and mount it on another development machine.

If the development machine does not have a copy of the repository on it already then git clone can be used.

```bash
git clone /path/to/memory/stick/repoName.git
```

If there is a copy of the repository already on the machine add the memory stick as a remote and fetch/pull the changes.

```git
cd /path/to/local/repo/
git remote add origin /path/to/memory/stick/repoName.git
git pull origin
```

From now on use Git as normal but make sure that whenever a git pull, fetch, or push is performed the memory stick is mounted on the machine.

### git bundle

A git bundle allows for part or all of a repository to be compressed into a single file in a format that git is able to clone and fetch from.

The workflow works very similar to before, but instead of copying the whole repository directory a git bundle is created. On the first machine create a bundle using:

```bash
git bundle create repoName.bundle --all
```
The ` -- all` option bundles the entire repository including all branches and tags. Specific branches or tags can be selected using `-b` branchName or `-t` tagName.

Copy the repoName.bundle file to another computer. To clone the repository simply use:

```bash
git clone repoName.bundle
```

Changes and commits can be made on any of the computers then like before one machine must be selected to perform the merge. On the non-merging machine make sure all changes are committed and create a bundle using:

```bash
git bundle create repoName.bundle --all
```

For larger repositories it is a good idea to only bundle part of the repository to avoid transferring more data than needed. For example to only include the last 5 commits on the master branch use:

```bash
git bundle create repoName.bundle -5 master
```

It is important that there are no gaps between the commits in the bundle and the commits on the repository where the merging will occur or the process will fail.

Copy the bundle to the computer where the merge will occur and pull the changes using:

```bash
git pull /path/to/repoName.bundle
```

Once the merging/rebasing is done create another bundle using:

```bash
git bundle create repoName.bundle --all
```

In the above command `--all` can be replaced with the desired subset of repos/commits.

Move the bundle file to the other machine/s and update the changes there using:

```bash
git pull /path/to/repoName.bundle
```

### Creating a local remote repository

Bundles solve the problem of synchronising Git repositories without a network, but we are still left with multiple computers all likely to be slightly out of sync with each other. If a new developer joins the team who do they copy the repository from? The best option is to select one development machine that will act as the “server”. A bare Git repository can be created on this development machine in addition to a local clone of the repository where the developer will actually work.

```bash
cd /path/to/store/main/repo
mkdir remoteRepoName.git
cd remoteRepoName.git
git init --bare
```

Next navigate to the local git repository or create a new one and add the remoteRepoName.git repository as a remote repository.

```bash
cd /path/to/local/repo/
git remote add origin /path/to/store/main/repo/remoteRepoName.git
git push origin branchName
```

Changes can then be made in the local repository, or pulled from bundles created on other development machines. Whenever changes are made they can be pushed to the remote using:

```bash
git push origin branchName
```

[source](https://gibbard.me/using_git_offline/)
