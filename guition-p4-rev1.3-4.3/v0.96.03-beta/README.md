# StajPilot v0.96.03-beta — Guition JC4880P443C_I_W

**This is a BETA.** It's newer and less battle-tested than the current
stable release ([v0.96.01](../v0.96.01)). If you don't need what's new
below, staying on v0.96.01 is the safer choice for now.

## What's new since v0.96.01

- **Fixed: Delay/Reverb could show the wrong on/off state when a
  downstream MIDI device (2-inch board, or a MIDI Captain/PySwitch unit)
  is connected through the board's USB Host bridge.** The downstream
  device's own connection could make the board briefly distrust a
  correct Delay/Reverb reading and get stuck showing it as off even
  though it was still on. Fixed by re-syncing just that specific
  tracking the moment the downstream device connects, without touching
  anything else on screen.
- **Firmware version now shown on the boot/waiting screen** (top-left) so
  it's always clear at a glance which version is running.

## Known limitations

- USB Host mode is newer and has had comparatively less real-world
  testing than the existing Device-mode Kemper Player connection, which is
  unchanged and continues to work exactly as before regardless of which
  USB mode you pick.
- DEVICE → HOST forwarding (both directions) has no message filtering —
  everything is passed through as-is.
- HOST mode enumerates MIDI-class USB devices; it hasn't been tested
  against every possible USB MIDI device, only what was available during
  development.
- Connection stability with other bidirectional MIDI controllers is still
  being tested — behavior may vary by device.

## Files

- `stajpilot_v0.96.03-beta_JC4880P443C_I_W_update_firmware.bin` — app-only
  update. Use this if you already have a working install - keeps your
  song list, WiFi settings, and other saved config.
- `stajpilot_v0.96.03-beta_JC4880P443C_I_W_factory_merged.bin` — full
  image (bootloader + partition table + app). Use this only for a first
  install or a full recovery - **this wipes saved song/WiFi config.**

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Guition JC4880P4 (BETA)" from the list there and follow the on-page
steps — they cover everything, including putting the board into upload
mode.

## Flashing: normal update (preserves song/WiFi config)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_v0.96.03-beta_JC4880P443C_I_W_update_firmware.bin
```

Flash offset `0x10000`. Does not write the partition table, does not
format SPIFFS/NVS.

## Flashing: factory install / recovery (wipes SPIFFS + NVS)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.96.03-beta_JC4880P443C_I_W_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x2000`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.
