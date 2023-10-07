# Printing JSON in the Shell

Printing json might seem straightforward at first, just:

```shell
$ echo '{"name": "jes", "sign": "aquarius"}'
# {"name": "jes", "sign": "aquarius"}
```

but inevitably, variables show up:

```shell
$ echo '{"name": "$name", "sign": "$sign"}'
# {"name": "$name", "sign": "$sign"} <-- this output is very broken
```

We can instead use `printf`! `printf` offers several advantages over echo:

- many languages (C, Go) have a `printf` equivalent, so it feels familiar
- `printf` behavior does not differ across systems (it is posix compliant)
- `printf` handles escape sequences (`printf "hello\nworld\n"`)
- `printf` can trivially handle json

Printing the same json text using `printf`:

```shell
$ printf '{"name": "%s", "sign": "%s"}' "$name" "$sign"
# {"name": "jes", "sign": "aquarius"}
```

[source](https://j3s.sh/thought/shell-tip-print-json-with-printf.html)
