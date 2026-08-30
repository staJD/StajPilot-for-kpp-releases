# StajPilot v0.95.00 — Waveshare ESP32-S3-Touch-LCD-2

<a href="https://youtu.be/kLVWh-ry95s" target="_blank">
  <img src="https://img.youtube.com/vi/kLVWh-ry95s/maxresdefault.jpg" alt="StajPilot on the Waveshare ESP32-S3-Touch-LCD-2" width="480">
</a>

## What's new in this version

- **First public beta for this board.**
- **BLE connection stability fix** — a background task creation failure
  could silently leave Bluetooth stuck in "CONNECT BUSY" or a
  perpetual scanning state, requiring a power cycle to recover. Fixed
  at the root cause, with the whole BLE stack rebuilt around a single
  unified connection-state worker task.
- **Dual-core task split** — MIDI I/O now runs on one core, and the
  display/touch/LVGL work on the other, coordinated with a
  non-blocking mutex. This is a large, directly-felt responsiveness
  improvement — the Kemper now reacts essentially instantly to
  on-screen input.
- **Bluetooth MIDI capped at 3 simultaneous connections** to keep
  internal-RAM headroom safe and connections reliable.
- **Wi-Fi and Bluetooth radios are mutually exclusive** — whichever one
  is actively in use takes priority, avoiding radio contention.
- **Morph gauge bar** — visualizes morph position with a red-to-blue
  gradient (red left, blue right) so you can see it at a glance.
- **LIQUID badge** on the rig display when a slot's amp is in Liquid
  Profile mode.
- **Zoomed display mode** for rig/song names, readable from a distance.
- **Song Mode Hangul + accented-Latin rendering fixes** — song names in
  Korean and accented Latin characters now render and wrap correctly.
- **Host mode support** — the USB-A port can be handed off to a second
  bi-directional MIDI device via a host-mode switch (needs a
  power-capable OTG adapter). Note: this adds load, since the Kemper
  then talks to two USB devices — a Bluetooth device is recommended
  instead where possible.
- **Bluetooth whitelist and MIDI-priority scanning** — pin the specific
  device(s) you use, and MIDI-capable devices surface first in the
  scan list.

## Files

- `stajpilot_v0.95.00_WS_S3_2_update_firmware.bin` — app-only update.
  Use this if you already have a working install - keeps your song
  list, WiFi settings, and other saved config.
- `stajpilot_v0.95.00_WS_S3_2_factory_merged.bin` — full image
  (bootloader + partition table + app). Use this only for a first
  install or a full recovery - **this wipes saved song/WiFi config.**

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Waveshare ESP32-S3 2.0" from the list there and follow the
on-page steps.

### Manual install with esptool

Normal update (preserves song/WiFi config):

```
esptool.py --chip esp32s3 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_v0.95.00_WS_S3_2_update_firmware.bin
```

Factory install / recovery (wipes SPIFFS + NVS):

```
esptool.py --chip esp32s3 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.95.00_WS_S3_2_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x0`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.
