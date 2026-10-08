# Install Log

## Pre-install decisions

### Distro choice — decided [today's date]
- Option A: Fedora Asahi Remix (officially supported on M3, safest first install)
- Option B: Asahi ALARM (Arch Linux ARM — the "real Arch" route, M3 support unconfirmed)
- Chosen: Asahi ALARM
- Why: I chose ALARM knowing that M3 Pro support is unconfirmed in this distro, the project is smaller and less tested than Fedora Asahi Remix, and the install may partially fail or produce a rougher result. I'm accepting this because my goal is learning, not a working daily driver on day one. I'll keep Asahi Remix as my backup if I cannot make ALARM work.

### Risk accepted
- M3 Pro support in ALARM: unconfirmed as of Oct 2026. Before running the installer,
  check https://asahi-alarm.org and/or their Matrix channel for M3 status.
- If the installer rejects the M3 Pro or fails: fall back to Fedora Asahi Remix,
  revisit ALARM later. This is not a failed project either way.

  ### Install command (verified Oct 2026)
- WRONG for ALARM: curl https://alx.sh | sh  (this installs FEDORA Asahi Remix)
- CORRECT for ALARM: curl https://asahi-alarm.org/installer-bootstrap.sh | sh
- M3 Pro support in ALARM: UNCONFIRMED. Matrix check pending.

### M3 hardware reality vs. YouTube claims — checked Oct 2026
- Some souces have said only HDMI + GPU are broken. Verified sources (Sept 2026 Asahi blog) say:
  GPU, HDMI, AND sleep are broken; brightness is partial. Sleep gap is framebuffer-
  related and expected to resolve with DCP support.
- None of these are user-fixable — they're missing drivers, upstream work only.
- Future milestone: replicate Omarchy's configs on top of ALARM (aarch64 repo exists),
  AFTER learning the layers manually. Not part of initial install scope.

  ### Matrix check (Oct 5, 2026) — @asahi-alarm:matrix.org
- Maintainer mkurz (Sep 11): "should work already, we pushed a new installer yesterday,
  but we do not have a M3 device at hand currently to test"
- User jane (Sep 20): Reports using "m3 pro 14 inch mbp" with active Bluetooth errors
- Conclusion: Installer will likely accept M3 Pro; not officially validated by ALARM team.
  Some users are successfully installed; expect untested quirks.

    ## Installation Day — Oct 7, 2026
    
    ### Pre-flight cleanup
    - Discovered 142GB free vs 163GB planned — traced to Downloads (21GB) + Docker data (17GB)
    - Docker CLI vanished: Docker Desktop was closed, CLI not on PATH; found it at
      /Applications/Docker.app/Contents/Resources/bin/docker
    - Found paused x200-kernel-builder container via bind mounts — source lived on
      Mac filesystem, NOT in container. No export needed.
    - Consolidated all X200 project files + Downloads to ~/Migration, uploading to Proton Drive
    - Deleted local copies, emptied Trash
    
    ### First install attempt — blocked (Oct 7)
    - Installer refused resize: 116GB overhead in local APFS snapshots
    - Purged local snapshots via tmutil deletelocalsnapshots → disk showed 343GB free → re-ran installer
    - STILL blocked: 330GB overhead. Cause: hourly TM snapshot regenerated +
      pending macOS update (MSUPrepareUpdate) pinning space.
    - Fix: delete new snapshot, disable TM, install macOS update first, then retry.
    
    ### Resize partition
    - Entering 120GB initially rejected (installer interprets as macOS size, needs 164GB minimum)
    - Entered 374GB for macOS → leaves 120GB free space for Linux
    - Partition resize SUCCESSFUL
    
    ### Boot ritual failures (first attempts)
    - Didn't hold power button long enough → booted back to macOS Recovery dialog
    - Selected wrong volume (macOS Recovery) → "version needs to be reinstalled" error
    - Correct boot: Hold power button until "Loading startup options..." → select Nightshift
    
    ### Security pairing (step2.sh)
    - Logged in as macOS user `williamingram` with macOS password (twice)
    - Security mode set to Permissive for Nightshift slot only
    - macOS security untouched
    
    ### First boot into Linux
    - Kernel: 7.1.13-3-2-ARCH aarch64 GNU/Linux
    - Login prompt shows `alarm login:` — default user `alarm`, password `alarm`
    - WiFi boot spam: brcmf -52 errors (M3 Pro Broadcom chip), connection works via nmtui
    - Interface: wld0 connected to "Home" WiFi network
    
    ### Sudo rescue (same evening)
    - `alarm` user NOT in sudoers — `sudo` rejected password attempts
    - Booted into recovery: held Esc during boot, edited GRUB entry, added `init=/bin/bash`
    - Ran as root:
      - `passwd root` (set temporary root password)
      - `usermod -aG wheel alarm` (add alarm to sudo group)
      - Edited `/etc/sudoers`: uncommented `%wheel ALL=(ALL) ALL`
    - Rebooted, logged in as `alarm`, `sudo whoami` returned `root` — FULL ADMIN CONFIRMED
    
    ---
    
    ## GUI Build — Hyprland Stack (same evening)
    
    ### Packages installed (~192 total, 957 MiB)
    - hyprland, firefox, kitty, wofi, waybar, sddm
    - wl-clipboard, cliphist, polkit-gnome (authentication)
    - papirus-icon-theme, dunst (optional)
    - Pacman lock errors (ran two instances simultaneously) — resolved naturally
    
    ### First graphical login
    - SDDM login screen: blue, user `ALARM`, date Oct 8 2026
    - Session: Hyprland (default)
    - Welcome wizard ran: parked on workspace 3 via Super+3 (Super+1 to return)
    - Clipboard verified: `echo test | wl-copy && wl-paste` prints `test`
    
    ---
    
    ## First Commands from Linux (Oct 7, 2026, ~9pm)
    
    ```bash
    uname -a                    # Linux alarm 7.1.13-3-2-ARCH aarch64 GNU/Linux
    nmcli device status         # wld0 connected to Home WiFi
    ping -c 4 1.1.1.1           # 0% packet loss
    sudo whoami                 # root ✓
    echo "test" | wl-copy       # clipboard test
    wl-paste                    # outputs "test"


