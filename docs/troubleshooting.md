### Git Push Fails with "setting up channel" / GUI Popup (Oct 8, 2026) 

**Symptom:** `git push` hangs indefinitely or spawns a GUI password dialog that never accepts input. No terminal prompt appears. 

**Root cause:** Fresh Hyprland session lacks a working GUI credential helper. Git's `askpass` helper tries to spawn a password window but fails silently, causing the infinite loop. 

**Fix:** Force Git to fall back to terminal prompts instead of GUI popups: 

```bash 
unset GIT_ASKPASS SSH_ASKPASS 
git push

---

### One-line paste mangling — clipboard loses newlines (Oct 9, 2026)

**Symptom:** Multi-line text pasted into nano or the shell arrives as one single line. Verification: wl-paste | wc -l prints 1 after copying a multi-line block.

**Cause (suspected):** Copying rendered HTML from the browser (Lumo web app in Firefox) on Wayland flattens newlines before they reach the clipboard. The clipboard stores the flattened text faithfully.

**Workarounds:**
- Type short edits by hand
- Use printf commands with embedded \n for file dumps
- One config edit at a time, with reload verification between each

**Status:** Not permanently fixed. Diagnostic pending: copy multi-line text from a terminal source, then run wl-paste | wc -l — if it prints more than 0, Firefox rendering is confirmed as the flattening layer.

---

### Hyprland config corruption — emergency mode recovery (Oct 9, 2026)

**Symptom:** Red error banners (nil value for hl, C stack overflow), then emergency mode with only Super+Q / Super+R / Super+M working.

**Root cause chain:** Multiple hand-edits accumulated in a 12KB Lua config — missing quotes on the wofi line, hyperland vs hyprland typo, a bind split across two lines. Each fix attempt introduced new errors.

**Fix:** rm ~/.config/hypr/hyprland.lua, then a full logout and relogin (Super+M to exit, SDDM login). Hot reload did NOT regenerate the missing config — a fresh session was required.

**Lessons:**
- One edit, save, reload, test. Never batch config edits.
- Command strings in binds need quotes: hl.dsp.exec_cmd("wofi --show drun")
- The config lives in the hidden directory ~/.config/hypr, not ~/config/hypr
- Fresh regen beats forensic repair for a config with four small intended changes
