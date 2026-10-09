# Asahi Linux on Apple Silicon M3 Pro

Field notes from installing Asahi ALARM on a late-2023 14-inch MacBook Pro.

Written in real-time, mistakes included. Not a tutorial. My experience, not official guidance.

## Hardware
- MacBook Pro 14-inch, Late 2023
- Apple M3 Pro, 18GB RAM, 512GB storage
- ~172GB free space at install time (October 2026)
- Time Machine backup: COMPLETE (encrypted, 2TB drive)

## Distro Choice
- Chosen: Asahi ALARM
- Reason: I chose ALARM because there is less support than Asahi Remix and i am looking for an ongoing project for a learning experience not an out of the box off the shelf experience. I am looking forward to and fully expect troubleshooting and debugging

## Known Limitations (M3 Pro, October 2026)
- GPU acceleration: NOT WORKING (CPU-rendered graphics only)
- Sleep/suspend: NOT WORKING
- HDMI port: NOT WORKING
- Display brightness: WORKING but under active development

## License
MIT — use this work, learn from it, tell me I was wrong where applicable.

    ## Documentation
    
    - **[Install Log](docs/install-log.md)** — Complete timeline from pre-install decisions through GUI boot, sudo rescue, and Git setup
    - **[Troubleshooting](docs/troubleshooting.md)** — Issues encountered + fixes (Git PAT auth, pacman locks, etc.)
    - **[Hardware Status](docs/hardware-status.md)** — Component test results (keyboard, trackpad, audio, sleep, HDMI)
    - **[What I Knew Before Starting](docs/what-i-knew-before-starting.md)** — Risk assessment and pre-install reasoning
    - **[Checkpoint Tracker](docs/checkpoint.md)** — Quick state snapshots, pending tasks, lessons learned
