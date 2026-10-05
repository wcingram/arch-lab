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