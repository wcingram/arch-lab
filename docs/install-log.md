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