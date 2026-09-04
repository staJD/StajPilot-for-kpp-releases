# StajPilot v0.96.01 — Guition JC4880P443C_I_W

> **Updated 2026-08-29**: the Song Mode text now supports accented
> characters used in Italian, Spanish, German, and Portuguese (ñ, á, ü,
> etc.) - previously these showed as blank boxes. Same v0.96.01
> version, binaries below reflect this fix.

This is now the current stable release, replacing v0.95.00. It carries
everything from the v0.96.00/v0.96.01 beta cycle, including USB Host
mode - see "Known limitations" below for that feature's current maturity.

## What's new since v0.96.00

- **DEVICE → HOST is now two-way** — a downstream MIDI device connected to
  the Host port can send data back, and it's now forwarded straight
  through to the Kemper Player (no filtering — everything the device sends goes
  through, like a standard MIDI THRU). Previously this direction was
  dropped entirely.
- **FINE-TUNE mode (new)** — a **FINE-TUNE** checkbox in Settings switches
  the tuner bar to real sub-cent precision (not just a finer-looking
  display — the underlying deviation value itself is now kept as a
  fraction of a cent) with a three-color band: in-tune (green), close
  (yellow), off (red/blue).
- **Faster rig-change response** — touching a rig or bank on the board no
  longer updates its own display state ahead of the Kemper Player's
  confirmation, which was causing a duplicate detail request on every
  change. The board now waits for the Kemper Player's own confirmation before
  asking for rig details — slightly later to update, but noticeably less
  load on the Kemper Player and a snappier feel overall.
- **Morph indicator polish** — visual redesign, a CPU usage bug fixed
  (it was doing unnecessary re-render work on every poll reply even when
  the value hadn't changed), and a rendering bug fixed (a stale-pixel
  artifact when the indicator's fill direction flipped).
- **Bluetooth "MIDI only" filter (new)** — a checkbox on the Bluetooth
  screen narrows the scan list to devices advertising BLE-MIDI (plus a
  name-based fallback for footswitch controllers that don't advertise it
  properly). Off by default.
- **Bluetooth connection-count indicator (new)** — a small strip above
  the Settings button fills in with one segment per currently connected
  BLE MIDI device.

## What's new in v0.96.00 (still included)

Everything below has accumulated since v0.95.00 - v0.96.00 was the first
public build to include any of it.

- **Swipe to change bank** — swipe left/right anywhere from the rig
  name down to the bottom of the screen to move to the previous/next
  bank, instead of using dedicated `</>` buttons. This frees up width for
  bigger slot buttons. A **SWIPE NAV** checkbox in Settings turns this
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
- **USB Host mode** — the board's Full-Speed USB-C port can act as a USB
  MIDI **Host**, so it can connect directly to the Kemper Player's own USB-B
  device port. This was built to let you connect other USB MIDI devices
  to the board - both one-way (send-only) and two-way (bidirectional)
  devices are supported. Three boot-fixed modes, selectable from a **USB
  MODE** page (accessible from the waiting screen before a Kemper Player is
  connected, or from Settings):
  - **DEVICE** (previous/default behavior) — High-Speed port as a USB MIDI
    Device, connected to the Kemper Player's USB-A host port.
  - **HOST** — Full-Speed port as a USB MIDI Host, connected to the
    Kemper Player's USB-B device port. Frees up the Kemper Player's own USB-A host port
    for a real foot controller.
  - **DEVICE → HOST** — both at once: receives from the Kemper Player as a
    Device on the High-Speed port and forwards MIDI out through the
    Full-Speed Host port to a second connected MIDI device — and (new as
    of this release) forwards that device's own replies back to the
    Kemper Player too.

  Changing the mode only saves the setting — the board applies it on the
  next power-on, it's never switched live while running.
- **Morph position indicator** — a small live display of the Kemper Player's
  morph pedal position, next to the big bank/rig number. Off by default
  when idle; only appears when the value actually changes, so it doesn't
  clutter the screen. A **MORPH** setting (None / Slow / Med / Fast)
  controls how often it checks — None turns polling off entirely if you
  don't use morph and would rather not have the extra MIDI traffic.

  **Note:** this works by polling the Kemper Player on a timer (Slow / Med
  / Fast), adding a small amount of continuous MIDI traffic
  even when nothing changes — the higher the speed, the more frequent the
  polling. Unless you actually use the morph pedal and want to watch its
  position on screen, we'd recommend setting this to **None**.

## Known limitations

- USB Host mode is newer and has had comparatively less real-world
  testing than the existing Device-mode Kemper Player connection, which is
  unchanged from v0.95.00 and continues to work exactly as before
  regardless of which USB mode you pick.
- DEVICE → HOST forwarding (both directions) has no message filtering —
  everything is passed through as-is.
- HOST mode enumerates MIDI-class USB devices; it hasn't been tested
  against every possible USB MIDI device, only what was available during
  development.
- Connection stability with other bidirectional MIDI controllers is still
  being tested — behavior may vary by device.

## Files

- `stajpilot_v0.96.01_JC4880P443C_I_W_update_firmware.bin` — app-only
  update. Use this if you already have a working install - keeps your
  song list, WiFi settings, and other saved config.
- `stajpilot_v0.96.01_JC4880P443C_I_W_factory_merged.bin` — full
  image (bootloader + partition table + app). Use this only for a first
  install or a full recovery - **this wipes saved song/WiFi config.**

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Guition JC4880P4" from the list there and follow the on-page
steps — they cover everything, including putting the board into upload
mode.

## Flashing: normal update (preserves song/WiFi config)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_v0.96.01_JC4880P443C_I_W_update_firmware.bin
```

Flash offset `0x10000`. Does not write the partition table, does not
format SPIFFS/NVS.

## Flashing: factory install / recovery (wipes SPIFFS + NVS)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.96.01_JC4880P443C_I_W_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x2000`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.
