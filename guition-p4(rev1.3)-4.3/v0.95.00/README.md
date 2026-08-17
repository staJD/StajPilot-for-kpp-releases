# StajPilot v0.95 — Rebrand + Boot Splash + WiFi UI Fixes

## What this version is

Builds on `v0.93ppacomplete` (PPA fix, amp-art revert, reveal frame-density
reduction, diagnostics removed). This version covers everything since then:
the FX reveal-animation on/off setting, WiFi password UI fixes, and a full
product rename from "StagePilot" to "StajPilot" across the touchscreen boot
splash, the WiFi config web page, the WiFi AP name, and the BLE device name.

## Why the rename (StagePilot → StajPilot)

A trademark check turned up multiple existing uses of "StagePilot" in
adjacent categories - a registered US mark (Arcivr, LLC, event-promotion/
business services, so lower direct conflict for a hardware product) and,
more relevantly, live-production software at `stagepilot.live` /
`stage-pilot.com` (lighting/audio/video control for shows - conceptually
close to this device). "Staj" is a phonetic stylization of "Stage" that
keeps the same pronunciation, paired with "Pilot" (kept over "Hub" -
"Hub" read as a passive relay/repeater device, "Pilot" better matches the
fact that the user actively drives the touchscreen in real time).
`StajPilot` itself came back clean in the same search pass. Not a full
legal clearance - fine for a personal project, worth a real trademark
search before any commercial use.

## New: FX reveal-animation on/off setting

New checkbox in Settings, left of SLOT SUB ("FX ANIM"). Turns the
effector-icon reveal animation (the frame-by-frame "on" transition baked
by `tools/fx_icon_bake`) on or off at runtime. When off, the icon jumps
straight to its final "on" frame instead of animating through the baked
sequence - a real CPU lever for players who don't care about the reveal
motion. Persisted via `preferences` (`fx_icon_anim` key), same pattern as
`amp_art`/`slot_sub`.

## WiFi password UI fixes

Three separate real bugs, found and fixed together:

1. **Textbox jitter** - `wifi_pass_ta`'s own `LV_OBJ_FLAG_SCROLLABLE` plus
   a too-tight height (42px against a 22pt font) triggered LVGL's default
   internal scrollbar, which nudged up/down. Fixed: height 42→46→58px
   (final size chosen so the box no longer looks cramped), scrollable flag
   removed - this is a one-line field, it never needs to scroll internally.
2. **Settings-modal auto-scroll** - separately, `settings_modal` itself was
   the only container in `gui.cpp` that still had `LV_OBJ_FLAG_SCROLLABLE`
   set (every other container in the file already removes it) - so
   focusing the password field triggered LVGL's default
   focus-scroll-into-view on the whole modal, looking like the modal itself
   was "moving". Fixed by removing the flag right after creation.
3. **Keyboard covering the field / wrong keys** - the stock numeric
   keyboard covered the password field while typing and included
   +/-/./cursor keys that don't apply to a digits-only field. Replaced
   with a custom `LV_KEYBOARD_MODE_USER_1` map (digits + backspace + OK
   only, no +/-, no comma), resized to 420×210, and repositioned/enlarged
   per iteration to bottom-left so the field stays visible while typing.

## StajPilot rebrand details

- **Boot splash (touchscreen)**: previously a baked bitmap of the old
  "StagePilot" wordmark plus a 6-milestone dot tracker tied to real
  `setup()` init steps. Replaced with:
  - A wordmark image that is the *actual cropped pixels* of the approved
    reference mockup (not a re-rendered font - a locally-available font
    didn't visually match the reference, so the real screenshot the user
    had saved to the working folder was cropped directly and baked as an
    ARGB8888 LVGL image asset, alpha derived from pixel brightness so it
    blends into any background). See `boot_wordmark.c/.h`.
  - A 13-dot progress row - count, dot size (8×8px), and color (`#1B7AE9`)
    all measured directly from that same reference image, not guessed.
  - The dots do **not** track real init milestones anymore - `setup()`'s
    own steps (SPIFFS mount, LVGL init, song-config PSRAM load, etc.) all
    finish in well under a second, and worse, none of it was ever visibly
    flushed to the panel during `setup()` in the first place (LVGL only
    renders/flushes from `lvgl_glue_handler()`, which only runs once
    `loop()` starts) - so a milestone-driven design either finished
    invisibly or showed nothing changing. The actual variable-length wait
    is for the Kemper to answer over USB MIDI, which this device has no
    way to estimate progress on. So the dots are now a plain indeterminate
    animation: `lv_anim_t` sweeps 0→13 left-to-right over 1.3s, pauses
    250ms, and repeats for as long as the loading layer is on screen.
  - Old dead asset removed: `stagepilot_boot_logo.c` (4.6MB baked bitmap,
    zero remaining references) deleted from the project.
- **WiFi config web page** (`/` on the device's own AP): header logo
  swapped to the same cropped reference image (`stagepilot_web_logo.c`,
  variable/file names left as-is - internal identifiers, not user-visible),
  page `<title>` and brand text updated to "StajPilot".
- **WiFi AP name**: `CONFIG_WIFI_SSID` "StagePilot" → "StajPilot" - this is
  the network name a phone connects to when WiFi config mode is turned on.
- **BLE device name**: `BLEDevice::init("StagePilot")` →
  `BLEDevice::init("StajPilot")` - the identity this device presents when
  acting as a BLE central toward other MIDI gear.
- **mDNS (`stajpilot.local`)** - added via `ESPmDNS`
  (`MDNS.begin("stajpilot")` + `MDNS.addService("http","tcp",80)`,
  `MDNS.end()` on AP stop). **Confirmed not resolving in testing** (Mac
  client, direct IP `192.168.4.1` works fine, `.local` does not). This
  device's WiFi is not native - the ESP32-P4 has no WiFi of its own and
  proxies everything through a companion ESP32-C6 over ESP-Hosted
  (`reset_c6_wifi_slave_for_ap_start()`), and mDNS-in-pure-AP-mode is
  already a known flaky area on stock single-chip ESP32 per multiple
  espressif/esp-idf and arduino-esp32 GitHub issues - the extra
  host↔co-processor layer here plausibly makes it worse, though no
  specific documented bug for this exact P4+C6 combination was found.
  **Recommendation: use `192.168.4.1` directly**; `mDNS.begin()` is left
  in place (harmless if it silently fails) rather than ripped out, in case
  a future ESP-Hosted/esp-idf update improves it.

## Not in this build: MIDI preset system

Designed but not implemented this session - a planned feature to store up
to 50 MIDI presets (each up to 10 messages, freely PC/CC/SysEx), fire them
to an **external device** (not the Kemper) whenever this controller's own
active bank/rig position changes, attached per song-slot via a new select
box in the web song-config page. Confirmed design decisions so far:
existing Kemper-facing bank/rig-select signals stay unchanged, the new
preset's messages fire *additionally* alongside them. Paused on: how the
external device physically connects (BLE reuse vs new BLE link vs
physical MIDI OUT vs other) - user still deciding, structure to be built
once that's settled.

## Web config: security pass + input limits

User asked directly whether the web config server could take the whole
device down if it misbehaved, and separately whether any input string
could break it. Both were checked against the actual code rather than
assumed:

- **Task isolation confirmed**: `config_server_task` and
  `config_wifi_worker_task` run as separate FreeRTOS tasks pinned to core
  0, deliberately split from the `loop()` task that drives MIDI/LVGL - a
  web-handler bug can't directly stall MIDI/rendering. The one real shared
  point of failure: a handler that acquires `lvgl_glue_lock()` and never
  releases it (crash mid-critical-section) would hang the main loop too.
  Any hard crash anywhere reboots the whole chip (one SoC), which is
  disruptive mid-show but never a permanent brick - re-flashing over
  serial always works regardless of firmware state.
- **XSS**: every user-text field (song/slot/sub/file names) is passed
  through `html_attr_escape()` (`&`/`"`/`<`/`>`) before being echoed back
  into the page - checked every `{{...}}` substitution site, all but the
  two pure-integer ones are escaped.
- **JSON injection** (`/api/bank`): `json_escape()` covers quotes,
  backslashes, and control characters.
- **Path traversal**: file names go through `sanitize_file_name()`
  (alnum-only), bank numbers are always `constrain()`-ed to 1-125 before
  being used to build a path - can't escape the SPIFFS directory.
- **Buffer overflow**: `copy_utf8_to_buffer()` bounds-checks and silently
  truncates on a UTF-8 character boundary (never mid-multibyte-sequence) -
  can't be overflowed by a long input.
- **Gap found and closed**: SONG and SLOT name fields had no length cap
  (SUB was already capped at 5 chars, FILE at 12) - unbounded input was
  safe at the storage layer (truncates cleanly) but had no cap at the
  point of entry. Added `maxlength="30"` (song) / `maxlength="20"` (slot)
  to the HTML form, and matching `limit_utf8_chars()` calls server-side in
  both `handle_config_bank_form_post()` and `handle_config_bank_post()`
  (the HTML-form and JSON-API POST paths both write the same storage, both
  needed the same fix).

## SD card (amp art): confirmed graceful when absent/rejected

Two separate questions, both traced through the actual code rather than
assumed:

- **No SD card present**: `amp_art_preload_task` (a background task, not
  the main loop) tries mounting once, waits 2.5s after boot first. On
  failure it sets `g_amp_art_sd_failed = true` permanently. Every later
  amp-art load attempt (`amp_art_try_load_key()`) checks that flag first
  and returns immediately - `SD_MMC.open()` is never called again after
  the first failure. The LVGL FS driver's own `open_cb` independently
  guards on `g_amp_art_sd_ready` too, so it's checked twice.
- **Oversized/wrong-size image files on the card**: `amp_art_scan_dir()`
  filters by exact byte size (`f.size() != AMP_ART_BYTES`, i.e. exactly
  386×140×2 = 108,080 bytes for a raw headerless RGB565 dump) before ever
  reading a file, and `amp_art_cache_file()` checks again at read time - a
  wrong-size file is skipped with a log line, never read into the
  fixed-size PSRAM buffer, so it can't overflow it. The caller
  (`amp_art_refresh_for_current_slot()`) treats a missing/rejected entry
  the same as "no art for this key": clears the image source and hides
  the art area, no broken image, no crash.

## Amp art cache limit raised 64 → 100

`AMP_ART_CACHE_MAX` 64 → 100 (`gui.cpp`). The scan loop
(`while (g_amp_art_cache_count < AMP_ART_CACHE_MAX)`) already stopped
cleanly once the cap was hit - only the cap number changed. Each cached
entry is ~105.5KB in PSRAM (386×140 RGB565); worst case (100 distinct amp
art files actually present) is ~10.5MB versus 32MB total PSRAM, still
comfortable alongside the display framebuffers and other PSRAM caches.

## Build fix: BIN_PACKAGE was leaking into the compile

Creating this backup's `source/` folder inside the project tree exposed a
real bug: `platformio.ini` had `src_dir = .` with no exclusion for
`BIN_PACKAGE/`, so PlatformIO's default source globbing picked up the
nested copy of `lvgl_v8_port.cpp` under `BIN_PACKAGE/v0.95stajpilot/source/`
and tried to compile it too, breaking the build. Fixed by adding
`-<BIN_PACKAGE/>` to `build_src_filter` - future backups with bundled
source snapshots won't hit this.

## Source included this time

Unlike earlier `BIN_PACKAGE` entries (bins + README only), this one also
includes a full source snapshot under `source/` - a straight copy of the
project working tree, excluding `.pio` (build cache), other
`BIN_PACKAGE/*` entries, `.DS_Store`, and the `fx_icon_bake` tool's own
compiled binary/build/preview output. This is prep for the GitHub
repo split planned next session: a private repo for the working
source, a public repo for distributable `.bin` files + manuals only.

## Build info

- LVGL: 9.5.0, `LVGL_GLUE_PARTIAL_REFRESH=1`
- Flash usage: ~7.20MB app (42.9% of the 16MB flash, app partition 10MB)
- RAM usage: ~99.7KB internal (30.4%)
- Compiled: 2026-08-17 (this snapshot includes the web-config security
  pass, SD-card verification, and amp art cache bump - see sections above)

## Files

- `stajpilot_v0.95_update_firmware.bin` — app only, for a normal update on
  an existing device.
- `stajpilot_v0.95_factory_merged.bin` — bootloader + partition table +
  app, for a first install or recovery.
- `source/` — full source snapshot (see above).

## Flashing: normal update (preserves song/WiFi config)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x10000 stajpilot_v0.95_update_firmware.bin
```

Flash offset `0x10000`. Does not write the partition table, does not
format SPIFFS/NVS.

## Flashing: factory install / recovery (wipes SPIFFS + NVS)

```
esptool.py --chip esp32p4 --port <PORT> --baud 460800 \
  write_flash 0x0 stajpilot_v0.95_factory_merged.bin
```

Flash offset `0x0`. Bootloader at `0x2000`, partition table at `0x8000`,
app at `0x10000`, merged into one image. Only for first install/recovery.

## Next steps (if revisited)

- Set up the two-repo GitHub split (private source, public
  releases/manuals) - discussed, not yet created.
- Decide the MIDI preset system's output transport, then build the
  storage/UI/trigger-hook design already agreed on.
- If `stajpilot.local` matters enough to chase further: try
  `mdns_register_netif()` to explicitly register the AP (C6-backed)
  interface with the ESP-IDF `mdns` component, though success isn't
  guaranteed given the known AP-mode flakiness.
