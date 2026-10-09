# Hardware Compatibility Log — M3 Pro

Tested components, dates, methods used, results.

| Component | Tested | Date | Status | Notes |
|-----------|--------|------|--------|-------|
| Keyboard  |        |      |        |       |
| Touchpad  |        |      |        |       |
| Webcam    |        |      |        |       |
| Mic       |        |      |        |       |
| Speakers  |        |      |        |       |
| WiFi      |        |      |        |       |
| Bluetooth |        |      |        |       |
| Sleep     |        |      |        |       |
| Display   |        |      |        |       |
| USB       |        |      |        |       |
| Video decode |    |      |        |       |

    # Hardware Status — M3 Pro / Asahi ALARM
    
    **Date:** Oct 8, 2026, ~10pm (ongoing testing)
    **Kernel:** 7.1.13-3-2-ARCH aarch64 GNU/Linux
    **Display Server:** Hyprland (Wayland)
    
    ## Tested Components (as of Oct 8, 2026)
    
    | Component | Status | Notes |
    |-----------|--------|-------|
    | Keyboard (built-in) | ✅ Working | Native keymap, includes Cmd/Super |
    | Trackpad | ✅ Working | Multi-touch gestures functional |
    | WiFi (Broadcom BCM43xx) | ✅ Working | Boot spews `brcmf -52` errors (cosmetic, connection succeeds) |
    | Bluetooth | ⚠️ Partial | Detected, some users report errors; untested personally |
    | Webcam | ❓ Untested | Need to test |
    | Microphone | ❓ Untested | Need to test |
    | **Speakers / Audio** | ❌ **Not Working** | **YouTube plays video with no audio — audio subsystem needs investigation** |
    | USB-C ports | ❓ Untested | Need to test |
    | External display (HDMI/DP) | ❌ Known failure | Per Asahi M3 status: HDMI not working |
    | Sleep | ❌ Known failure | Per Asahi M3 status: sleep broken |
    | Brightness controls | ⚠️ Partial | Per Asahi M3 status: limited support |
    
    ## Audio Investigation (Oct 8, 2026, ~10pm)
    
    **Symptom:** Video plays on YouTube but produces no audible output through built-in speakers. Headphone jack also silent (unverified — needs plug-in test).
    
    **Initial hypotheses:**
    - PipeWire not running or misconfigured (expected for fresh Arch — no auto-setup like Ubuntu)
    - Wrong output device selected in PulseAudio/PipeWire volume control
    - ALSA mixer muted at hardware level
    - Missing firmware or kernel modules for Apple Silicon audio controller
    
    **Diagnostic commands to run next:**
    ```bash
    # Check PipeWire status
    systemctl --user status pipewire pipewire-pulse wireplumber
    
    # List audio sinks
    pw-cli ls Node  # or pactl list sinks if using PulseAudio
    
    # Check if any device shows up
    arecord -l   # playback devices
    aplay -l     # capture devices
    
    # Unmute ALSA master channel
    amixer sset Master unmute
    amixer sset Speaker unmute
