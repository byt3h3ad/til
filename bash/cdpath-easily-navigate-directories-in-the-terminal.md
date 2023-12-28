# CDPATH: Easily Navigate Directories in the Terminal

The CDPATH environment variable lets you pre-configure frequently used directories so you can easily navigate into them from anywhere in your file system[^1].

A `cd` search path works like your command search path `$PATH`. However, instead of finding commands, it finds the subdirectories. You can configure it with the shell variable `CDPATH`, and it has the same format as the `PATH` variable: a list of directories separated by colons.

Set this variable in your `~/.zshconfig` or `~/.bashrc` file to include the directories you visit most frequently.

```bash
export CDPATH=$HOME:$HOME/software:$HOME/software/js:$HOME/software/web
```
Now, whenever you want to `cd` into a directory, the shell will look through all the above places in addition to the current directory.

What’s more, the search is lightning fast. It looks only in parent directories that you specify and nothing else.

For example, let’s say you have a `$HOME/software/blog` directory and you’ve configured the CDPATH to include the `$HOME/software` directory.

Now, if you type `cd blog` from anywhere in the filesystem, the cd command will take you to the `$HOME/software/blog` directory, unless it finds another `blog` directory in another pre-configured path.

> The order of `CDPATH` matters. If two directories in `$CDPATH` have a subdirectory named blog, the earlier parent wins.

In the above example, `cd` will check the existence of the following directories in order, until it finds one or it fails.

1. in the current directory

2. `$HOME/software/blog`

3. `$HOME/software/js/blog`

4. `$HOME/software/web/blog`

P.S. Don't overdo it. If you put too many directories with common names into CDPATH and `export` it, it might break your scripts.

[source](https://learning.oreilly.com/library/view/efficient-linux-at/9781098113391/)

[^1]: [`z`](https://github.com/rupa/z) does a great job of achieving the same outcome by tracking your most used directories, based on ['frecency'](https://web.archive.org/web/20210421120120/https://developer.mozilla.org/en-US/docs/Mozilla/Tech/Places/Frecency_algorithm). After  a  short  learning  phase, z will take you to the most 'frecent' directory that matches ALL of the regexes given on the command line, in order. Rather elegant, and I use it all the time.
