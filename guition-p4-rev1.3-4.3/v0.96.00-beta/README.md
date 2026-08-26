# StajPilot v0.96.00-beta — Guition JC4880P443C_I_W

**This is a BETA.** It's newer and less battle-tested than the current
stable release ([v0.95.00](../v0.95.00)). The headline new feature (USB
Host mode) is genuinely new hardware/firmware territory for this project —
if you don't need it, staying on v0.95.00 is the safer choice for now.

## What's new in this version

Everything below has accumulated since v0.95.00 - this beta is the first
public build to include any of it.

- **Swipe to change bank (new)** — swipe left/right anywhere from the rig
  name down to the bottom of the screen to move to the previous/next
  bank, instead of using dedicated `</>` buttons. This frees up width for
  bigger slot buttons. A new **SWIPE NAV** checkbox in Settings turns this
  off again if you'd rather keep the old buttons - nothing is lost either
  way.
- **Second, longer slot label ("sub swipe")** — when SWIPE NAV is on, each
  slot button can show a longer secondary label (up to 12 characters)
  instead of the short 5-character one, since swipe mode frees up the
  width for it. Set per-slot from the web config page; falls back to the
  short label automatically if left blank.
- **MIDI CHANNEL control** changed from a scrolling roller to a dropdown,
  matching the BOX/FIXED FX controls.
- **Web config page**: column widths on the song/slot editor now track
  character count directly instead of stretching unevenly, and a couple
  of small label/wording fixes.

- **USB Host mode (new)** — the board's Full-Speed USB-C port can now act
  as a USB MIDI **Host**, so it can connect directly to the Kemper's own
  USB-B device port. This was built to let you connect other USB MIDI
  devices to the board - both one-way (send-only) and two-way
  (bidirectional) devices are supported. Three boot-fixed modes,
  selectable from a new **USB MODE** page (accessible from the waiting
  screen before a Kemper is connected, or from Settings):
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

  **Note:** this works by polling the Kemper on a timer (Slow 600ms / Med
  400ms / Fast 200ms), adding a small amount of continuous MIDI traffic
  even when nothing changes — the higher the speed, the more frequent the
  polling. Unless you actually use the morph pedal and want to watch its
  position on screen, we'd recommend setting this to **None**.

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
