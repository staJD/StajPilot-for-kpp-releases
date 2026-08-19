# StajPilot v0.95.00 — Guition JC4880P443C_I_W
<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt=“StajPilot width="480">
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

## Files

- `stajpilot_v0.95.00_JC4880P443C_I_W_update_firmware.bin` — app-only
  update. Use this if you already have a working install - keeps your
  song list, WiFi settings, and other saved config.
- `stajpilot_v0.95.00_JC4880P443C_I_W_factory_merged.bin` — full image
  (bootloader + partition table + app). Use this only for a first
  install or a full recovery - **this wipes saved song/WiFi config.**

## Flashing: normal update (preserves song/WiFi config)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_v0.95.00_JC4880P443C_I_W_update_firmware.bin
```

Flash offset `0x10000`. Does not write the partition table, does not
format SPIFFS/NVS.

## Flashing: factory install / recovery (wipes SPIFFS + NVS)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.95.00_JC4880P443C_I_W_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x2000`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.
