```
████████╗ ██████╗ ██╗  ██╗███████╗██╗
╚══██╔══╝██╔═══██╗██║ ██╔╝██╔════╝██║
   ██║   ██║   ██║█████╔╝ █████╗  ██║
   ██║   ██║   ██║██╔═██╗ ██╔══╝  ██║
   ██║   ╚██████╔╝██║  ██╗███████╗██║
   ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝
```

[![arb](https://img.shields.io/badge/arb-package-05d9e8?style=flat-square)](https://github.com/MenkeTechnologies/arb)
![type](https://img.shields.io/badge/self--sourcing-dashboard-9b5de5?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-ff2a6d?style=flat-square)

### `LINES OF CODE BY LANGUAGE, RECOUNTED LIVE`

> *"Watch your codebase's language breakdown redraw itself every ten seconds."*

A self-sourcing dashboard over [`tokei`](https://github.com/XAMPPRocky/tokei): it runs `tokei` on a timer and tables the per-language line counts, so nothing needs to be piped in — the dashboard feeds itself. It is an `arb` package — a dashboard for [`arb`](https://github.com/MenkeTechnologies/arb), the pipeline-to-TUI language on the fusevm bytecode VM.

### [`arb`](https://github.com/MenkeTechnologies/arb) &middot; [`arb-registry`](https://github.com/MenkeTechnologies/arb-registry)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Usage](#0x02-usage)
- [\[0x03\] The Spec](#0x03-the-spec)
- [\[0x04\] How It Works](#0x04-how-it-works)
- [\[0xFF\] License](#0xff-license)

## [0x00] OVERVIEW

`tokei` renders the language-by-language line-count report from the `tokei` code-statistics tool as a live table. Point it at a repository, leave it open, and the per-language breakdown re-tallies on its own timer as you edit — no manual re-runs, no piping.

## [0x01] INSTALL

```sh
arb install tokei
arb search tokei
```

## [0x02] USAGE

```sh
arb tokei                       # run it (self-sourcing; refreshes on its own)
arb tokei --serve               # browser dashboard
arb tokei --html > tokei.html   # static HTML
```

## [0x03] THE SPEC

```arb
# tokei — Lines of code by language from tokei, self-sourced from `tokei`.
! tokei every 10s
table .tokei
source .tokei { in }
grid .tokei -row 0 -col 0
```

- `! tokei every 10s` — the self-source: it runs the `tokei` command every 10 seconds and feeds its stdout into the `.tokei` stream, so the dashboard supplies its own data.
- `table .tokei` — declares a table widget bound to the `.tokei` stream, rendering the language rows and their counts.
- `source .tokei { in }` — the query pipeline for the stream; `in` takes the raw self-sourced input and passes it through to the widget.
- `grid .tokei -row 0 -col 0` — places the `.tokei` widget at row 0, column 0 of the dashboard grid.

## [0x04] HOW IT WORKS

The `!` self-source runs `tokei` on its 10-second timer and pushes each run's stdout into the `.tokei` stream. The `source .tokei { in }` pipeline shapes that input, and the `table` widget renders it into the grid cell at row 0, column 0. arb lowers the pipeline's compute to the fusevm bytecode VM with Cranelift JIT.

## [0xFF] LICENSE

MIT.
