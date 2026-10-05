# StajPilot v0.96.03-beta — Guition JC4880P443C_I_W

> [!CAUTION]
> **Guition P4 (4.3"): please don't buy this board for StajPilot right now.**
>
> I've heard sellers are switching to shipping **rev3.2** boards, which use a
> newer ESP32-P4 chip revision (v3.x). The current StajPilot firmware is built
> for **rev1.3** only, and Espressif states that v1.x and v3.x chips cannot run
> the same firmware.
>
> To prevent malfunctions on rev3.2 boards, Guition P4 firmware installation
> on the firmware site ([stajd.github.io](https://stajd.github.io/)) is closed
> for now.
>
> Once I get a rev3.2 board, I'll post a new notice here. That will take **at
> least a month**, and I can't promise rev3.2 support.

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

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/#guition-jc4880p4-beta)

That link opens directly on "Guition JC4880P4 (BETA)" - follow the
on-page steps, they cover everything including putting the board into
upload mode.
