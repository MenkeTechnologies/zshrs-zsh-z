```
███████╗███████╗██╗  ██╗     ███████╗
╚══███╔╝██╔════╝██║  ██║     ╚══███╔╝
  ███╔╝ ███████╗███████║█████╗ ███╔╝ 
 ███╔╝  ╚════██║██╔══██║╚════╝███╔╝  
███████╗███████║██║  ██║     ███████╗
╚══════╝╚══════╝╚═╝  ╚═╝     ╚══════╝
                                     
```

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![zshrs plugin](https://img.shields.io/badge/zshrs-native%20plugin-blue.svg)](https://github.com/MenkeTechnologies/zshrs)

### `[FRECENCY DIRECTORY JUMPER — COMPILED]`

> *"z <partial> — the frecency jump, reimplemented in Rust."*

## `[NATIVE ZSHRS PLUGIN]`

[zsh-z](https://github.com/agkozak/zsh-z) — the frecency directory jumper (`z <partial>` cd's to the directory you visit most, weighted by how recently and how often) — ported to a **native [zshrs](https://github.com/MenkeTechnologies/zshrs) plugin**. A faithful reimplementation in Rust: the `~/.z` datafile format, the frecency formula, the aging rule, the matching, and the `z` options are all reproduced.

### [`zshrs`](https://github.com/MenkeTechnologies/zshrs) &middot; [`znative`](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZPM.md) &middot; [`upstream`](https://github.com/agkozak/zsh-z)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Usage](#0x02-usage)
- [\[0x03\] How it works](#0x03-how-it-works)
- [\[0xFF\] License](#0xff-license)

---

## [0x00] OVERVIEW

```text
z proj          → cd to the highest-frecency dir matching "proj"
z -l proj       → list matches with scores
z -e proj       → echo the best match (don't cd)
z -c src        → restrict to subdirs of $PWD
z -r / z -t     → rank / recency ordering instead of frecency
z -x            → forget the current directory
```

---

## [0x01] INSTALL

```sh
znative load MenkeTechnologies/zshrs-zsh-z
```

Put that one line in your `.zshrc`. [znative](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZPM.md), zshrs's package manager, installs the plugin on the first shell start — clones it, runs `cargo build --release`, and `zmodload -R`s the resulting `libzsh_z` — then loads it from the store, zero-network, on every start after. No separate install step; then `z <dir>` jumps.

### Manual build

```sh
cargo build --release
zmodload -R ./target/release/libzsh_z.dylib   # .so on Linux
z <partial-dir>
```

---

## [0x02] USAGE

`z <partial>` jumps to the best-matching directory; the flags above list, echo, restrict, re-order, or forget. Directory tracking is automatic once the plugin is loaded.

---

## [0x03] HOW IT WORKS

The datafile (`$ZSHZ_DATA`, default `~/.z`) holds `path|rank|time` rows. Frecency is `rank * (3.75/(0.0001*dx + 1) + 0.25)` with `dx = now - time`; each visit bumps rank, ranks age `*0.99` past `$ZSHZ_MAX_SCORE` (9000), and non-existent dirs are pruned — identical to zsh-z. Directory tracking is a `chpwd` hook (`z --add "$PWD"`) the plugin installs on first `z` use; the actual `cd` is delegated to the shell so `$PWD`/hooks stay correct.

---

## [0xFF] LICENSE

MIT. Ported from [agkozak/zsh-z](https://github.com/agkozak/zsh-z) (MIT). See [LICENSE](LICENSE).
