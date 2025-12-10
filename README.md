# Remap Copilot Key to Right Ctrl (AutoHotkey v2)

This repo shows how to turn the **Copilot key** on a Windows laptop into a **Right Ctrl** key using **AutoHotkey v2**.

The script is made for the case where the Copilot key sends:  
`Win + Shift + F23`  
(That’s what it does on my machine, checked via AutoHotkey’s key history.)

---

## What this script does

- When you **hold the Copilot key**, it behaves like **Right Ctrl**.
- So:
  - `Copilot + C` → works like `Ctrl + C`
  - `Copilot + V` → works like `Ctrl + V`
  - `Copilot + Backspace` → works like `Ctrl + Backspace`
  - `Copilot + Arrow keys` → works like `Ctrl + Arrow` (jump word by word)

- It also fixes an annoying issue where the Copilot key was acting like:
  - `Ctrl + Shift + C`
  - `Ctrl + Shift + Backspace`
  - etc.

  That happened because **Win + Shift** stayed “held down” in the background.  
  The script explicitly **releases Win and Shift** before pressing Right Ctrl.

---

## 🚀 Features

- Turns **Copilot key → Right Ctrl**
- Cancels hidden **Win + Shift** modifiers
- Works consistently across:
  - Browsers (Chrome, Edge, Firefox)
  - Text editors (VS Code, Notepad, etc.)
- Supports holding the key as a proper modifier:
  - `Copilot + C` → `Ctrl + C`
  - `Copilot + Backspace` → `Ctrl + Backspace`
  - `Copilot + Arrow` → `Ctrl + Arrow` (move by word)

---

## 📦 Requirements

- Windows 10 / 11  
- AutoHotkey **v2** (important — v1 syntax will NOT work)

---

## Check what your Copilot key sends

You can check it like this:

1. Create and run a simple AutoHotkey v2 script (`test.ahk`):
   ```ahk
   #Requires AutoHotkey v2.0
   KeyHistory()
   ```
2. Press the **Copilot key** a few times.
3. Open the **AutoHotkey icon** in the system tray → **Open** → press **Ctrl + K**.
4. Look at the last few lines.

On my system, pressing the Copilot key produced: `LWin`, `LShift`, `F23`.

---

## 📜 Script (remap.ahk)

```ahk
#Requires AutoHotkey v2.0
#SingleInstance Force

; optional: Ctrl + Shift + Alt + R reloads this script
^+!r::Reload()

; remap the Copilot key (Win + Shift + F23) to behave like Right Ctrl
*<+<#F23::
{
    ; release Shift + Win so they don’t interfere (fixes Ctrl+Shift issue)
    Send("{Blind}{LShift up}{LWin up}{RControl down}")

    ; wait until the copilot key is released
    KeyWait("F23")

    ; then release rtCtrl
    Send("{RControl up}")
}
```

---

## 🔍 Script Explanation (Line‑by‑Line)

### `#Requires AutoHotkey v2.0`
Ensures the script only runs under AHK v2.  
This prevents errors from v1 syntax compatibility.

### `#SingleInstance Force`
Stops duplicate running scripts.  
If the script is already running, it reloads instead of starting a second instance.

### `^+!r::Reload()`
Adds a manual reload hotkey: **Ctrl + Shift + Alt + R**

Useful when modifying the script.

---

## 🔑 Main Remap Section

### `*<+<#F23::`
This hotkey fires when:
- `*` → works with any extra modifiers  
- `<+` → left Shift  
- `<#` → left Win  
- `F23` → hidden keycode triggered by Copilot

Together, this matches the **actual signal** the Copilot key sends.

---

### Inside the block:

#### `Send("{Blind}{LShift up}{LWin up}{RControl down}")`
- `{Blind}` preserves only the keys you explicitly change  
- `LShift up` releases Shift  
- `LWin up` releases Win  
- `RControl down` holds Right Ctrl

This is the critical fix that stops accidental **Ctrl+Shift** shortcuts.

---

#### `KeyWait("F23")`
Waits until the Copilot/F23 key is physically released.  
This allows the key to behave like a proper modifier.

---

#### `Send("{RControl up}")`
Releases Right Ctrl when the Copilot key is released.

---

## ▶️ How to Use

1. Install AutoHotkey v2.
2. Create a file named `remap.ahk`.
3. Paste the script into it.
4. Double-click to run.
5. Test:
   - Copilot + C  
   - Copilot + Backspace  
   - Copilot + Arrow keys  

Everything should behave exactly like holding **Ctrl**.

---

## 🔁 Auto‑Start on Windows

1. Press `Win + R`
2. Type:
   ```
   shell:startup
   ```
3. Place a shortcut to `remap.ahk` inside that folder.

Windows will run the script automatically on boot.

---

## 💬 Notes

- This script assumes your Copilot key sends `Win + Shift + F23`.  
- If your laptop uses a different pattern, check with `KeyHistory()` and adjust accordingly.

---

## ✔️ Summary

This script restores sanity to the Copilot key.  
Instead of triggering unwanted browser shortcuts or selecting text unexpectedly, it becomes a **clean, reliable, real Ctrl key** usable everywhere.

Enjoy the productivity boost!
