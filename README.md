# c3270 on macOS

A **practical, terminal-native setup** for running **c3270** on macOS using **Terminal.app**, with a classic green-screen look, reliable key mappings, oversize screens, and working copy & paste.

This repository documents a **working, tested configuration**. It is intentionally opinionated and macOS-specific.

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/810572ec-03ef-4c08-a21e-47ec876012c4" />

---

## Motivation

Getting **c3270** to work *well* on macOS is harder than expected:

- key mappings behave differently than on Linux/X11
- many Ctrl/Alt combinations are impossible in a terminal
- copy & paste appears "broken" out of the box
- default colors and fonts do not resemble a real 3270
- documentation rarely covers Terminal.app specifics

After extensive experimentation, this setup provides:

- stable keyboard behavior in **Terminal.app**
- reliable Enter / Newline / Backspace handling
- classic green-on-black appearance
- optional oversize screens (e.g. 90×32)
- working mouse-based copy & paste

---

## Contents

- `c3270pro` – example configuration (copy to `~/.c3270pro`)
- `3270.terminal` – Terminal.app profile (colors, cursor, ANSI palette)
- this README

---

## Requirements

- macOS
- Homebrew
- Terminal.app (this setup explicitly targets Terminal.app)

---

## Installation

### 1. Install an IBM 3270 font

```sh
brew install --cask font-3270
```

The font must be selected manually later in Terminal.app. It is **not embedded** in the profile.

---

### 2. Install c3270

```sh
brew install x3270
```

Despite the package name, modern macOS systems only install **c3270** (the curses-based emulator). The X11 GUI `x3270` is not used.

---

### 3. Install the c3270 configuration

```sh
cp c3270pro ~/.c3270pro
```

`.c3270pro` is a hidden file in your home directory.

---

### 4. Import the Terminal.app profile

1. Open **Terminal.app**
2. Settings / Preferences → **Profiles**
3. Import `3270.terminal`
4. Select the imported profile
5. Set the font manually:
   - Font: **3270**
   - Size: **18** (recommended)

---

## Screen Size Configuration

This setup uses an **oversize screen** while staying compatible with 3270 semantics.

Recommended:

```text
c3270.model: 3
c3270.oversize: 90x32
```

- Model 3 = 80x32 base
- Terminal window must be at least 90×32 characters

---

## Mouse Selection & Copy / Paste (Important)

### The Problem

By default, **copy & paste appears broken** in c3270:

- no visible mouse selection
- ⌘C copies nothing
- ⌘V pastes nothing

This is **not a c3270 bug**.

### The Cause

Terminal.app has a global runtime option:

```
View → Allow Mouse Reporting
```

When this is **enabled**, Terminal.app forwards mouse events to applications. In that mode, **Terminal.app disables text selection entirely**.

Fullscreen applications like `c3270`, `vim`, or `tmux` often trigger this.

---

### The Solution

In **Terminal.app**:

```
View → Allow Mouse Reporting
```

→ **Disable this option** (no checkmark)

After disabling:

- mouse selection is visible again
- ⌘C copies selected text
- ⌘V pastes into c3270 input fields
- ⌥ + mouse drag enables rectangular selection

> Note: This is a **global menu option**, not a profile setting.

---

## Paste Handling

Commonly referenced resources:

```text
c3270.marginedPaste
```

is **not supported** by c3270 and results in a warning.

```text
c3270.overlayPaste
```

is **not supported** by c3270 and results in a warning.

---

## Key Bindings (Defined in `.c3270pro`)

The following table documents the **effective key bindings** and their behavior. All bindings are chosen to be **terminal-safe**.

### Input & Editing

| Key         | Action           | Effect                |
| ----------- | ---------------- | --------------------- |
| Enter       | `Enter()`        | Submit screen         |
| Alt + Enter | `Newline()`      | Insert newline        |
| Ctrl + J    | `Newline() Up()` | Newline and cursor up |
| Backspace   | `Erase()`        | Delete character left |
| Insert      | `ToggleInsert()` | Toggle insert mode    |
| Alt + I     | `ToggleInsert()` | Insert mode fallback  |
| End         | `FieldEnd()`     | Move to end of field  |
| Alt + E     | `EraseEOF()`     | Erase to end of field |

### Navigation

| Key         | Action      | Effect              |
| ----------- | ----------- | ------------------- |
| Home        | `Home()`    | Move cursor to home |
| Ctrl + H    | `Home()`    | Home (fallback)     |
| Shift + Tab | `BackTab()` | Previous field      |
| Alt + Z     | `BackTab()` | BackTab fallback    |

### PF / PA Keys

| Key              | Action  | Effect              |
| ---------------- | ------- | ------------------- |
| Page Up          | `PF(7)` | Scroll up           |
| Page Down        | `PF(8)` | Scroll down         |
| Ctrl + A, then 1 | `PA(1)` | Program Attention 1 |
| Ctrl + A, then 2 | `PA(2)` | Program Attention 2 |
| Ctrl + A, then 3 | `PA(3)` | Program Attention 3 |

### System Actions

| Key      | Action     | Effect              |
| -------- | ---------- | ------------------- |
| Alt + C  | `Clear()`  | Clear screen        |
| Alt + A  | `Attn()`   | Attention key       |
| Ctrl + G | `Redraw()` | Redraw screen       |
| Ctrl + R | `Reset()`  | Reset keyboard lock |

---

## Notes on Terminal Limitations

- Many Ctrl combinations are indistinguishable in terminals
- `Ctrl+I = Tab`, `Ctrl+M = Enter`, `Ctrl+J = Linefeed`
- `Ctrl+1`, `Ctrl+2`, `Ctrl+3` do not exist as real terminal keys

This keymap avoids impossible combinations and uses proven fallbacks.

---

## Disclaimer

This is an opinionated, macOS-specific configuration. It favors reliability, clarity, and long interactive sessions over maximal portability.

---

## License

Do whatever you want with it.

