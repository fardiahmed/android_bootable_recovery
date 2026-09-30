# A16_RISCV/bootable/recovery: android16-riscv

Changes made for the Android 16 (AOSP, riscv64) bring-up of the BananaPi BPI-F3 (SpacemiT K1) and the BananaPi BPI-SM10 (SpacemiT K3), on branch `android16-riscv`.

Recovery/fastbootd on the BPI-F3 HDMI display.

## Changes

- **minui: prefer HDMI and never pick a writeback connector**: Writeback connectors report connected with fabricated modes, so a board whose display is HDMI (BananaPi F3) fell through to the first-connected fallback and could bind recovery to writeback: every flip succeeded but nothing reached the screen. Add HDMI-A/B to the main-screen list and skip writeback in the fallback.
- **minui: skip connectors without an encoder/CRTC when disabling CRTCs**: Unused DSI or writeback connectors have no CRTC; dereferencing it crashed recovery on boards exposing several connectors.
- **minui: log only the selected display mode**: Printing every mode exhausted the /dev/kmsg ratelimit before recovery's own log lines.
- **recovery_ui: make the long-press time configurable**: ro.recovery.ui.long_press_ms (default 750 ms) sets how long a key must be held to select on button-limited devices.

## Build

```
./build.sh k1   # BPI-F3 (K1): kernel, bootloaders and Android images
./build.sh k3   # BPI-SM10 (K3), not booted yet
```

`./build.sh --help` lists the options (`--skip-kernel`, `--kernel-only`, ...).
