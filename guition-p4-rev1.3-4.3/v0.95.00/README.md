# StajPilot v0.95.00 — Guition JC4880P443C_I_W

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
<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt="StajPilot" width="480">
</a>

## What's new in this version

- **FX reveal-animation on/off setting** — new "FX ANIM" checkbox in
  Settings, next to SLOT SUB. Turns off the effector-icon reveal
  animation, for extra CPU headroom if you don't need the animation.
- **WiFi password screen fixes** — the password field no longer jitters,
  the on-screen keyboard no longer covers the field while typing, and
  it's now a clean digits-only keypad (no unused +/-/comma keys).
- **Song/slot name length limits** — song names capped at 30 characters,
  slot names at 20, to keep the on-screen display tidy.
- **Amp art capacity raised to 100 images** (up from 64) on the microSD
  card.
- **Web config server hardened** — input validation and length limits
  added across the WiFi song-config page.
- **More robust against a missing/misformatted SD card** — verified the
  device falls back cleanly with no crash if the card is absent or a
  file doesn't match the expected image format.
- **Fixed an audible backlight whine** — the screen's backlight dimming
  circuit was switching at 5kHz, right in the middle of audible range.
  Raised to 25kHz (above what people can hear). Brightness control
  itself is unchanged. This may also reduce noise picked up through
  your audio signal chain when the device is connected alongside other
  gear.

## Known limitation

- `stajpilot.local` (mDNS) doesn't reliably resolve on this board yet -
  use the direct IP `192.168.4.1` for the WiFi config page instead.

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Guition JC4880P4" from the list there and follow the on-page
steps — they cover everything, including putting the board into upload
mode.
