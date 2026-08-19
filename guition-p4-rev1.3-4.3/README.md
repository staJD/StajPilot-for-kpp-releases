# StajPilot — Guition JC4880P443C_I_W (rev1.3)

<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt=“StajPilot width="480">
</a>


Touchscreen MIDI controller for the Kemper Profiler Player, built on an
ESP32-P4 + ESP32-C6 (Guition JC4880P443C_I_W, 4.3" 800×480 touch LCD).

Currently only tested against the Kemper Profiler **Player** - not
verified against the other Profiler models (Stage, PowerHead, Rack).

## Features

- Bank/rig navigation with 5 performance slots + tap tempo + tuner
- Live effect on/off display per slot, with animated reveal icons
  (togglable if you'd rather save CPU)
- Amp art display, loaded from a microSD card
- **Song Mode** - the Kemper Profiler Player has no concept of songs of
  its own, only banks/slots. StajPilot adds a simple Song Mode on top of
  that: a WiFi config page lets you label each bank/slot with a song
  name, sub-label, and amp art file, so the same bank/slot navigation
  you already use also shows song info - no need to touch the device's
  own screen to edit it. This is just per-slot labeling, not a curated,
  reorderable setlist feature.
- Bluetooth MIDI support for wireless foot controllers - tested with 3x
  M-VAVE Chocolate pedals connected simultaneously, up to 5 supported

## MIDI communication with the Kemper

StajPilot runs in the Kemper's bi-directional mode and relies as much as
possible on the data the Kemper streams automatically, rather than
polling for it - Kemper's own guidance recommends avoiding heavy request
traffic while in bi-directional mode. Some detailed data isn't part of
that auto-stream and still needs a direct request; those requests are
kept optional where possible, so users can choose to skip them. The
whole MIDI layer is built around stability and keeping load on the
Kemper's own system low - the streamed data itself stays real-time,
this is about not piling extra requests on top of it.

## Getting the firmware

Grab the latest release for this board from the
[Releases](../../../releases) page (tagged
`VX.XX.XX_stajPilot(JC4880P443C_I_W)`). Each release
includes:

- `stajpilot_vX.XX.XX_JC4880P443C_I_W_update_firmware.bin` — app-only update. Use this if
  you already have a working install and just want the new version -
  keeps your song list, WiFi settings, and other saved config.
- `stajpilot_vX.XX.XX_JC4880P443C_I_W_factory_merged.bin` — full image (bootloader +
  partition table + app). Use this only for a first install or a full
  recovery - **this wipes saved song/WiFi config.**

Each release's notes describe what changed in that version.

## Flashing

Needs [esptool](https://github.com/espressif/esptool)
(`pip install esptool`).

**Normal update** (keeps your song list/WiFi config):

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_vX.XX.XX_JC4880P443C_I_W_update_firmware.bin
```

**Factory install / recovery** (first install, or full reset):

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_vX.XX.XX_JC4880P443C_I_W_factory_merged.bin
```

`<PORT>` is the device's USB serial port (e.g. `/dev/cu.usbmodem*` on
Mac, `COM*` on Windows).

## Hardware

- Guition JC4880P443C_I_W, rev1.3
- ESP32-P4 + ESP32-C6 WiFi/BT companion
- 4.3" 800×480 RGB touch LCD
- microSD card slot for amp art images (optional - the device works
  without one)

## Song Mode config

The Kemper Profiler Player only knows about banks and slots - Song Mode
is StajPilot's own addition, letting you label that same bank/slot
structure by song instead. Turn on WiFi config mode from the device's
Settings screen, connect your phone or computer to the `StajPilot` WiFi
network it broadcasts, then open `192.168.4.1` in a web browser - a
computer's keyboard makes it a lot faster if you're entering a lot of
songs at once. From there you can set
song names, per-slot labels, and which amp art file to show for each
bank - your existing bank/slot navigation then also shows that song
info. There's no separate, reorderable setlist - it's just a label on
each bank/slot.
