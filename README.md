# AmigaChrome Application Ports

Open-source application ports and compatibility work for **AmigaOS 3.x**, **m68k AROS** and **AmigaChrome**: the sister of [amigachrome-gameports](https://github.com/DalsinAI/amigachrome-gameports), for programs that are not games.

## Current port state

None yet.

## Licences

Each port keeps the licence of the program it ports, recorded beside it. The Team's own build scripts and patches are MIT licensed and free, Copyright (c) 2026 Dalsin Limited.


## Working documents

- [APPLICATION_PORT_BACKLOG.md](APPLICATION_PORT_BACKLOG.md) — unified application backlog and priority board.
- [PRIOR_ART.md](PRIOR_ART.md) — AROS, classic AmigaOS, AmigaOS 4 and MorphOS prior art worth mining before starting fresh portability work.

## Porting rule

Before writing new Amiga portability code, check AROS Contrib/Ports, Aminet, OS4Depot and MorphOS Storage first. Prefer recovering proven Amiga-family work and replacing obsolete platform glue with Open-family interfaces over solving the same problem again.

Primary AmigaChrome target: **AC090 / AmigaOS 3.x / 68040 + FPU**.

Priority language used by the backlog:

- **P0** — finish / enable now
- **P1** — next delivery wave
- **P2** — planned
- **P3** — stretch
- **PARK** — research / low return for now


## Governing principle

Every port should answer this before work starts:

> **What need does this remove to use another platform for?**

The aim is not to collect ports for their own sake. The aim is to make AmigaChrome capable of completing real workflows end-to-end: media conversion, audio editing, graphics work, document handling, development, publishing, capture, storage and other everyday jobs without requiring a fallback to Linux, Windows, macOS or another machine.

This **platform-substitution value** is a primary prioritisation criterion alongside feasibility, effort, reuse value and Open-family platform value.
