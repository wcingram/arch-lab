### Git Push Fails with "setting up channel" / GUI Popup (Oct 8, 2026) 

**Symptom:** `git push` hangs indefinitely or spawns a GUI password dialog that never accepts input. No terminal prompt appears. 

**Root cause:** Fresh Hyprland session lacks a working GUI credential helper. Git's `askpass` helper tries to spawn a password window but fails silently, causing the infinite loop. 

**Fix:** Force Git to fall back to terminal prompts instead of GUI popups: 

```bash 
unset GIT_ASKPASS SSH_ASKPASS 
git push
