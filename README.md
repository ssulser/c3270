# c3270 on macOS

Stuff to get a working and nice-looking **c3270** emulator on macOS (Terminal.app).

## Motivation

I had a hard time getting **c3270** to work *well* and to look *right* on macOS.
After a lot of trial and error (and some help from ChatGPT), I ended up with a setup that is:

* stable and usable for long ISPF sessions
* keyboard-friendly (Terminal.app quirks handled)
* classic green-on-black look
* supports a larger screen (e.g. 90×32)

This repository documents my setup so others don’t have to rediscover it.

## Contents

* `c3270pro` — example configuration (copy to `~/.c3270pro`)
* `3270.terminal` — Terminal.app profile (green-on-black)
* notes and recommendations

## Requirements

* macOS
* Homebrew
* Terminal.app (this repo targets Terminal.app; iTerm2 will require different key handling)

## Setup

### 1) Install an IBM 3270 font

```
brew install --cask font-3270
```

You need to select the font manually in Terminal.app later (it is not embedded in the profile template).

### 2) Install c3270 (via x3270 package)

```
brew install x3270
```

Despite the name, on modern macOS this effectively gives you **c3270** (not the X11 GUI `x3270`),
because native X11 isn’t supported anymore (unless you go the XQuartz route, which I wanted to avoid).

### 3) Install the c3270 configuration

Copy `c3270pro` from this repository to your home directory and rename it to `.c3270pro`:

```
cp c3270pro ~/.c3270pro
```

Note: `.c3270pro` is a hidden file.

### 4) Import the Terminal.app profile and set the font

1. Open **Terminal.app**
2. Settings/Preferences → **Profiles**
3. Import `3270.terminal`
4. Select the imported profile
5. Set the font manually:

   * Font: `3270`
   * Size: `18` (recommended)

## Recommended screen size

A very comfortable setup for ISPF is:

* Model: `3` (32×80)
* Oversize: `90×32`

Example:

```
c3270.model: 3
c3270.oversize: 90x32
```

## Notes on key mappings (Terminal limitations)

Terminal-based key handling has real limitations:

* Some Ctrl combinations cannot be distinguished (e.g. `Ctrl+I` is Tab).
* `Ctrl+1`, `Ctrl+2`, `Ctrl+3` are not reliably available in terminals.
* Using `U+000D` / `U+0008` / `U+007F` mappings is often more robust than symbolic key names.

The provided keymap is designed specifically for **Terminal.app** and focuses on:

* Enter / Newline / Newline+Up
* Backspace as Erase
* BackTab, Clear, PF7/PF8
* Home, Insert, FieldEnd, EraseEOF
* PA1–PA3 (terminal-friendly bindings)

## Disclaimer

This is an opinionated, macOS-specific setup. If it saves you time and frustration, it did its job.

## License

Do whatever you want with it.
