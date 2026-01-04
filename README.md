# c3270 on macOS  
*A practical guide to a working and good-looking 3270 terminal on macOS*

## Motivation

I wanted to use **c3270** on macOS for classic mainframe work (ISPF, TSO, Hercules),
but getting it to work **properly** – and to look **right** – turned out to be harder than expected.

Typical problems I ran into:

- confusing key mappings in Terminal.app
- broken or inconsistent modifier keys (Ctrl / Alt / Meta)
- ugly default fonts
- no clear documentation for macOS-specific issues
- 3270 screen sizes behaving unexpectedly

After a lot of trial and error (and many hours of debugging), I ended up with a setup that is:

- ✅ stable
- ✅ keyboard-friendly
- ✅ visually close to a classic 3270 green screen
- ✅ usable for long ISPF sessions

This repository documents that setup so others don’t have to rediscover everything from scratch.

---

## What this repository contains

- `.c3270pro` – a tested c3270 configuration for macOS  
- `3270.terminal` – a Terminal.app profile (green-on-black, 3270-style)
- documentation on fonts, key mappings, and screen size
- a minimal, reproducible setup

---

## Requirements

- macOS
- Homebrew
- Terminal.app (not iTerm2 – this setup is explicitly for Terminal.app)

---

## Installation

### 1. Install an IBM 3270 font

```sh
brew install --cask font-3270
