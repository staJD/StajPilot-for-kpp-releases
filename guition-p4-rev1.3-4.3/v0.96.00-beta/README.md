# StajPilot v0.96.00-beta — Guition JC4880P443C_I_W

**This is a BETA.** It's newer and less battle-tested than the current
stable release ([v0.95.00](../v0.95.00)). The headline new feature (USB
Host mode) is genuinely new hardware/firmware territory for this project —
if you don't need it, staying on v0.95.00 is the safer choice for now.

## What's new in this version

- **USB Host mode (new)** — the board's Full-Speed USB-C port can now act
  as a USB MIDI **Host**, so it can connect directly to the Kemper's own
  USB-B device port. Three boot-fixed modes, selectable from a new **USB
  MODE** page (accessible from the waiting screen before a Kemper is
  connected, or from Settings):
  - **DEVICE** (previous/default behavior) — High-Speed port as a USB MIDI
    Device, connected to the Kemper's USB-A host port.
  - **HOST** — Full-Speed port as a USB MIDI Host, connected to the
    Kemper's USB-B device port. Frees up the Kemper's own USB-A host port
    for a real foot controller.
  - **DEVICE -> HOST** — both at once: receives from the Kemper as a
    Device on the High-Speed port, and forwards every MIDI event
    out through the Full-Speed Host port to a second connected MIDI
    device. One-way only in this release (Kemper -> downstream device);
    the reverse direction isn't built yet.

  Changing the mode only saves the setting — the board applies it on the
  next power-on, it's never switched live while running.

- **Morph position indicator (new)** — a small live display of the
  Kemper's morph pedal position, next to the big bank/rig number. Off by
  default when idle; only appears when the value actually changes, so it
  doesn't clutter the screen. New **MORPH** setting (None / Slow / Med /
  Fast) controls how often it checks — None turns polling off entirely if
  you don't use morph and would rather not have the extra MIDI traffic.

- **Settings screen cleanup** — "MIDI CHANNEL" shortened to "CHANNEL",
  the four dropdown settings (CHANNEL / BOX / MORPH / FIXED FX) tightened
  up to fit in one row, and the USB mode shortcut button removed from this
  screen (still reachable from the waiting screen, and from a dedicated
  entry point in Settings on the USB MODE page).

## Known limitations (beta-specific)

- USB Host mode is new and has had comparatively little real-world testing
  compared to the existing Device-mode Kemper connection, which is
  unchanged from v0.95.00 and continues to work exactly as before
  regardless of which USB mode you pick.
- DEVICE -> HOST forwarding is one-way only (Kemper -> downstream device).
  A downstream device sending MIDI back is not yet read or acted on.
- HOST mode enumerates MIDI-class USB devices; it hasn't been tested
  against every possible USB MIDI device, only what was available during
  development.

## Files

- `stajpilot_v0.96.00-beta_JC4880P443C_I_W_update_firmware.bin` — app-only
  update. Use this if you already have a working install - keeps your
  song list, WiFi settings, and other saved config.
- `stajpilot_v0.96.00-beta_JC4880P443C_I_W_factory_merged.bin` — full
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
  write_flash 0x10000 stajpilot_v0.96.00-beta_JC4880P443C_I_W_update_firmware.bin
```

Flash offset `0x10000`. Does not write the partition table, does not
format SPIFFS/NVS.

## Flashing: factory install / recovery (wipes SPIFFS + NVS)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.96.00-beta_JC4880P443C_I_W_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x2000`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.
