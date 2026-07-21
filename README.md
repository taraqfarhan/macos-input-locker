# macOS Input Locker
Temporarily locks and pauses keyboard (and optionally mouse/trackpad) input for a specified duration. It includes a safety kill switch key combination to unlock the system early. This program is useful for cleaning your Mac's keyboard/mouse/trackpad, preventing accidental key presses, or keeping curious friends (foes) from messing with your computer.

---

## Overview
`lk` pauses keyboard input (optionally mouse/trackpad clicks/scrolls/gestures) for a specified duration.
Safety kill switch: Press a custom key combination to unlock early. ("ctrl+u" is set as the default keybind)

```
Usage:
  lk <duration> [-c] [-e] [-k "ctrl+u"]
  lk -d "ctrl+q"   (sets a new permanent default keybind)

Duration formats:
  5      (5 seconds)
  10s    (10 seconds)
  2m     (2 minutes)
  1h     (1 hour)

Examples:
  # pause keyboard inputs for 60 seconds
  lk 60               

  # pause keyboard inputs for 2 minutes
  lk 2m           

  # pause keyboard, mouse and trackpad inputs/gestures/scrolls for 1 hour
  lk -e 1h            
  
  # pause keyboard input for 20 minutes while setting a temporary key "ctrl+q"
  lk -k "ctrl+q" 20m  
  
  # set a new permanent default keybind (saved in ~/.lk_config file)
  lk -d "ctrl+q"      

  # quit the terminal immediately after starting the lock while pausing keyboard input for 10 minutes
  lk -c 10m

  # quit the terminal immediately after starting the lock while setting a temporary key "ctrl+q"
  lk -c -k "ctrl+q" 10m

  # quit the terminal immediately after starting the lock while locking everything for 10 minutes
  lk -c -e 10m

Requirements:
  Accessibility permissions:
      System Settings > Privacy & Security > Accessibility
```
---

## Installation & Setup

1. **Make the script executable**:
   ```bash
   chmod +x lk
   ```

2. **Add to PATH**:
   To use the `lk` command globally, symlink it to a directory in your shell's path (such as `/usr/local/bin`):
   ```bash
   ln -s "$(pwd)/lk" /usr/local/bin/lk
   ```

3. **Enable Accessibility Permissions**:
   The first time the command runs, macOS may prompt you or log an error.
   - Open **System Settings > Privacy & Security > Accessibility**.
   - Ensure your terminal application (e.g., Terminal, iTerm2, VS Code, Kitty) is enabled in the list.

---

## Command Line Interface

```bash
lk <duration> [flags]
```

| Flag | Long Flag | Description |
|---|---|---|
| `-e` | `--everything` | Lock keyboard, mouse clicks, scrolls, and trackpad gestures |
| `-k KEY` | `--set-key KEY` | Override default unlock combination for this specific run |
| `-d KEY` | `--default KEY` | Permanently save a new default key combination to `~/.lk_config` |
| `-c` | `--close-terminal` | Quit the terminal immediately after starting the lock |

---

## How It Works Under the Hood

The script uses standard macOS C APIs dynamically loaded via Python's standard `ctypes` library:
- **`CGEventTapCreate`** (CoreGraphics): Registers a tap that captures and blocks selected system input events.
- **`CFRunLoopRun`** (CoreFoundation): Runs a loop on the main thread to listen to the event tap stream.
- **Daemonization**: Runs the Locker in a subprocess starting a new session (`start_new_session=True`), separating it from the parent shell.
