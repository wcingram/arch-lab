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

    ## Audio Investigation (Oct 8, 2026, ~10pm)
    
    **Status:** Audio sinks detected, investigating mute/configuration
    
    **Test results:**
    - `pactl list sinks short` shows **two sinks detected**:
      - S5 (HiFi_Speaker_sink): RUNNING
      - S6 (HiFi_Headphones_sink): SUSPENDED (expected when nothing plugged in)
    - Default sink: `alsa_output.platform-sound.HiFi_Speaker_sink`
    - **Diagnosis:** Hardware is recognized by PipeWire — issue is configuration (mute/volume/app routing), not driver
    
    **Actions taken:**
    - Installed `pipewire-pulse` + `rtkit` packages
    - Enabled all user services: pipewire, pipewire-pulse, wireplumber
    - Unmuted default sink: `pactl set-sink-mute @DEFAULT_SINK@ 0`
    - Set volume: `pactl set-sink-volume @DEFAULT_SINK@ 75%`
    
    **Next tests:**
    - [ ] Test with `sox` tone generator
    - [ ] Hard-refresh YouTube tab (Ctrl+Shift+R) and test
    - [ ] Verify Firefox sink input via `pactl list sink-inputs short`
    - [ ] If still silent, investigate ALSA mixer settings via `alsamixer`

    ### Audio Investigation Conclusion (Oct 9, ~9am)
    
    **Status:** ⚠️ **Partially Working / Driver Limitation**
    
    **Findings:**
    - Audio sinks detected by PipeWire (S5 speakers, S6 headphones)
    - Firefox successfully routes to sinks (sink-inputs confirmed)
    - ALSA mixer channels unmuted (`Speaker`/`Headphone` = on)
    - **Raw ALSA test fails:** `speaker-test -D plughw:0,0` returns `Transfer failed: Bad address`
    - **No Asahi audio kernel modules loaded:** `lsmod \| grep asahi` → empty
    - **Conclusion:** DSP/firmware path from kernel to Apple Silicon speakers is incomplete
    
    **What works (conceptually):**
    - PipeWire architecture is functional
    - Application-to-PipeWire routing works (Firefox streams audio)
    - ALSA device enumeration works
    
    **What breaks (implementation gap):**
    - Kernel driver can't push audio data to speaker hardware
    - This is a known Asahi Linux M3 limitation — driver development ongoing
    
    **Next steps:**
    1. [ ] Ensure `asahi-firmware` package installed
    2. [ ] Check for `asahi-audio-dsp` or similar kernel modules
    3. [ ] Monitor Asahi GitHub/Matrix for audio driver updates
    4. [ ] Temporary workaround: USB-C DAC or Bluetooth headphones (bypasses broken speaker path)
    5. [ ] Consider switching to HDMI/external speakers once HDMI support arrives
    
    **References:**
    - https://github.com/AsahiLinux/linux/issues (audio issues)
    - Asahi Linux Matrix channel (@asahi-alarm:matrix.org)
