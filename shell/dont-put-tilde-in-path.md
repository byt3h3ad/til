# Don't put a quoted tilde in PATH

Tilde expansion only happens on unquoted words. This does **not** expand `~`:

```sh
export PATH="$PATH:~/.local/bin/"
```

`PATH` then contains a literal `~/.local/bin/`, which is a relative entry resolving to `./~/.local/bin/` in the current directory. Since it is appended, it only catches commands not found earlier in `PATH`, but a writable `./~/.local/bin/` in any working directory can still get executed.

Use `$HOME` instead:

```sh
export PATH="$PATH:$HOME/.local/bin/"
```

The unquoted `export PATH=$PATH:~/.local/bin` works in Bash and Zsh (tilde is expanded after `=` and each `:` in assignments), but it relies on shell-specific assignment rules, so it is fragile and less portable.

Check for the problem:

```sh
echo "$PATH" | tr ':' '\n' | grep '~'
```

Source: [The tilde in your PATH may not be your HOME](https://disconnect3d.pl/2026/10/02/dont-put-tilde-in-your-path/)
