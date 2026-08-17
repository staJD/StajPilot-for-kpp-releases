# StajPilot — Guition JC4880P443C_I_W (rev1.3)

[← back to all boards](../)

Touchscreen MIDI controller for the Kemper Profiler Player, built on an
ESP32-P4 + ESP32-C6 (Guition JC4880P443C_I_W, 4.3" 800×480 touch LCD).

Built and tested specifically for the Kemper Profiler **Player** - not
verified against the other Profiler models (Stage, PowerHead, Rack).

## Features

- Bank/rig navigation with 5 performance slots + tap tempo + tuner
- Live effect on/off display per slot, with animated reveal icons
  (togglable if you'd rather save CPU)
- Amp art display, loaded from a microSD card
- Song/setlist config via a WiFi page the device hosts itself - map
  songs to bank/slot/sub-label/amp-art-file per bank, no need to touch
  the device's own screen to edit your setlist
- Bluetooth MIDI support for wireless foot controllers

## Getting the firmware

Grab the latest release for this board from the
[Releases](../../../releases) page (tagged `p4-vX.XX`). Each release
includes:

- `stajpilot_vX.XX_update_firmware.bin` — app-only update. Use this if
  you already have a working install and just want the new version -
  keeps your song list, WiFi settings, and other saved config.
- `stajpilot_vX.XX_factory_merged.bin` — full image (bootloader +
  partition table + app). Use this only for a first install or a full
  recovery - **this wipes saved song/WiFi config.**

Each release's notes describe what changed in that version.

## Flashing

Needs [esptool](https://github.com/espressif/esptool)
(`pip install esptool`).

**Normal update** (keeps your song list/WiFi config):

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_vX.XX_update_firmware.bin
```

**Factory install / recovery** (first install, or full reset):

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_vX.XX_factory_merged.bin
```

`<PORT>` is the device's USB serial port (e.g. `/dev/cu.usbmodem*` on
Mac, `COM*` on Windows).

## Hardware

- Guition JC4880P443C_I_W, rev1.3
- ESP32-P4 + ESP32-C6 WiFi/BT companion
- 4.3" 800×480 RGB touch LCD
- microSD card slot for amp art images (optional - the device works
  without one)

## Song/setlist config

Turn on WiFi config mode from the device's Settings screen, connect
your phone to the `StajPilot` WiFi network it broadcasts, then open
`192.168.4.1` in a browser. From there you can set song names, per-slot
labels, and which amp art file to show for each bank.
