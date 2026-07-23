# macOS input locker
Temporarily locks and pauses keyboard (and optionally mouse/trackpad) input for a specified duration. It includes a safety kill switch keybind (by default `ctrl+u`) to unlock the system early. 

This program is useful for cleaning mac's keyboard/mouse/trackpad, preventing accidental key presses, or keeping curious friends (or foes) from messing with your mac.

---

## Overview
`lk` pauses keyboard input (optionally mouse/trackpad clicks/scrolls/gestures) for a specified duration. Press a custom keybind to unlock early (`ctrl+u` is set as the default keybind)

```
Usage:
  lk [flags] <duration>
  lk [-c] [-e] [-k "cmd+shift+q"] <duration> 
  lk -d "ctrl+q"

Flags:
  -c         Close the terminal window immediately after starting the lock
  -e         Lock everything (keyboard, mouse clicks, scrolls, and trackpad gestures)
  -k <KEY>   Override default unlock keybind for this specific run 
  -d <KEY>   Permanently save a new default keybind to `~/.lk_config` 
             "ctrl+u" is used by-default without any modification
 
Duration formats:
  5      (5 seconds)
  10s    (10 seconds)
  2m     (2 minutes)
  1h     (1 hour)

Examples:
  # lock keyboard inputs for 60 seconds
  lk 60               

  # lock keyboard inputs for 2 minutes
  lk 2m           

  # lock everything (keyboard, mouse and trackpad) for 1 hour
  lk -e 1h            

  # close the terminal window immediately and lock keyboard inputs for 2 minutes
  lk -c 2m  
  
  # lock keyboard input for 20 minutes while setting a temporary key "ctrl+q"
  lk -k "ctrl+q" 20m  
  
  # set a new permanent default keybind (saved in ~/.lk_config file)
  lk -d "ctrl+q"      

  # we can also combine multiple flags 
  # lock everything & close the terminal window immediately
  lk -c -e 10m
  lk -ce 10m

  # lock everything, close the terminal window immediately
  # and set a temporary keybind for unlocking
  lk -c -e -k "ctrl+q" 10m
  lk -cek "ctrl+q" 10m  
```
---

## Installation & Setup
1. **Clone the repository and cd into the directory**:
   ```bash
   git clone https://github.com/taraqfarhan/macos-input-locker
   cd macos-input-locker
   ```

2. **Make the script executable**:
   ```bash
   chmod +x lk
   ```

3. **Add to PATH**:
   To use the `lk` command globally, symlink it to a directory in your shell's path (such as `/usr/local/bin`):
   ```bash
   ln -s "$(pwd)/lk" /usr/local/bin/lk
   ```

4. **Enable Accessibility Permissions**:
   The first time the command runs, macOS may prompt you or log an error.
   - Open **System Settings > Privacy & Security > Accessibility**.
   - Ensure your terminal application (e.g., Terminal, iTerm2, VS Code, Kitty) is enabled in the list.
