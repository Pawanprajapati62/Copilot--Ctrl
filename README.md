# Remap the Copilot Key to Right Ctrl (AutoHotkey v2)

This repo shows how to turn the **Copilot key** on a Windows laptop into a **Right Ctrl** key using **AutoHotkey v2**.

The script is made for the case where the Copilot key sends:  
`Win + Shift + F23`  
(That’s what it does on my machine, checked via AutoHotkey’s key history.)

---

## 1. What this script does

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

## 2. Requirements

- **Windows 10 / 11**
- **AutoHotkey v2** installed

---

## 3. Check what your Copilot key sends

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

## 4. The actual remap script

Create a file called **`remap.ahk`** and put this code inside:

```ahk
#Requires AutoHotkey v2.0
#SingleInstance Force

^+!r::Reload()

*<+<#F23::
{
    Send("{Blind}{LShift up}{LWin up}{RControl down}")
    KeyWait("F23")
    Send("{RControl up}")
}
```

### Explanation

- Releases **LShift** and **LWin** so the shortcut becomes clean.
- Holds **Right Ctrl** while Copilot is held.
- Releases Ctrl when Copilot is released.

---

## 5. How to run this script

1. Install AutoHotkey v2.
2. Save the script as `remap.ahk`.
3. Double-click it to run.
4. Test in Notepad and browser.

---

## 6. Make it run on startup

1. Press **Win + R**, type:
   ```
   shell:startup
   ```
2. Add a shortcut to your `remap.ahk` file.

---

## 7. Troubleshooting

- Make sure you're using AutoHotkey v2.
- Make sure the script starts with:
  ```ahk
  #Requires AutoHotkey v2.0
  ```
- If Copilot still opens Windows Copilot:
  - Your OEM might override the key.
  - Re-check the key codes using KeyHistory.

---

## My take

This is a clean and practical way to repurpose the Copilot key.  
The only caveat is OEMs may update how the key behaves, so occasionally re-check your key codes.
