# zshrs-zsh-z

[zsh-z](https://github.com/agkozak/zsh-z) — the frecency directory jumper
(`z <partial>` cd's to the directory you visit most, weighted by how recently
and how often) — ported to a **native
[zshrs](https://github.com/MenkeTechnologies/zshrs) plugin**. A faithful
reimplementation in Rust: the `~/.z` datafile format, the frecency formula,
the aging rule, the matching, and the `z` options are all reproduced.

```text
z proj          → cd to the highest-frecency dir matching "proj"
z -l proj       → list matches with scores
z -e proj       → echo the best match (don't cd)
z -c src        → restrict to subdirs of $PWD
z -r / z -t     → rank / recency ordering instead of frecency
z -x            → forget the current directory
```

## How it works

The datafile (`$ZSHZ_DATA`, default `~/.z`) holds `path|rank|time` rows.
Frecency is `rank * (3.75/(0.0001*dx + 1) + 0.25)` with `dx = now - time`;
each visit bumps rank, ranks age `*0.99` past `$ZSHZ_MAX_SCORE` (9000), and
non-existent dirs are pruned — identical to zsh-z. Directory tracking is a
`chpwd` hook (`z --add "$PWD"`) the plugin installs on first `z` use; the
actual `cd` is delegated to the shell so `$PWD`/hooks stay correct.

## Install

With **zpm** (zshrs's package manager):

```sh
zpm add MenkeTechnologies/zshrs-zsh-z
```

`zpm` clones, `cargo build --release`s the cdylib, and `zmodload -R`s it. Add
`zpm load zsh-z` to your `.zshrc` to load at startup. Then `z <dir>` jumps.

## Build manually

```sh
cargo build --release
zmodload -R ./target/release/libzsh_z.dylib   # .so on Linux
z <partial-dir>
```

## License

MIT. Ported from [agkozak/zsh-z](https://github.com/agkozak/zsh-z) (MIT). See
[LICENSE](LICENSE).
