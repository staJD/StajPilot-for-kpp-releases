# StajPilot — Guition JC4880P443C_I_W (rev1.3)

<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt="StajPilot" width="480">
</a>


Touchscreen MIDI controller for the Kemper Profiler Player, built on an
ESP32-P4 + ESP32-C6 (Guition JC4880P443C_I_W, 4.3" 800×480 touch LCD).

> ⚠️ **Before you buy: confirm the board revision with the seller.**
> This firmware is built and tested specifically against **rev1.3** of
> the Guition JC4880P443C_I_W. Other revisions can differ in pinout and
> components, and are **not guaranteed to work**. Ask the seller to
> confirm it's rev1.3 before you order.

Currently only tested against the Kemper Profiler **Player LV3** - not
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

## MIDI communication with the Kemper Player

StajPilot runs in the Kemper Player's bi-directional mode and relies as
much as possible on the data it streams automatically, rather than
polling for it - their own guidance recommends avoiding heavy request
traffic while in bi-directional mode. Some detailed data isn't part of
that auto-stream and still needs a direct request; those requests are
kept optional where possible, so users can choose to skip them. The
whole MIDI layer is built around stability and keeping load on the
Kemper Player's own system low - the streamed data itself stays
real-time, this is about not piling extra requests on top of it.

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Guition JC4880P4" from the list there and follow the on-page
steps — they cover everything, including putting the board into upload
mode.

### Manual install with esptool

If you'd rather flash it yourself from the command line:

1. Install [esptool](https://github.com/espressif/esptool):
   `pip install esptool`.
2. Grab the latest release for this board from the
   [Releases](../../../releases) page (tagged
   `VX.XX.XX_stajPilot(JC4880P443C_I_W)`). Each release includes:
   - `stajpilot_vX.XX.XX_JC4880P443C_I_W_update_firmware.bin` — app-only
     update. Use this if you already have a working install and just
     want the new version - keeps your song list, WiFi settings, and
     other saved config.
   - `stajpilot_vX.XX.XX_JC4880P443C_I_W_factory_merged.bin` — full
     image (bootloader + partition table + app). Use this only for a
     first install or a full recovery - **this wipes saved song/WiFi
     config.**
3. Put the board into upload mode: unplug the USB cable, press and hold
   the **BOOT** button, then plug the cable back in while still holding
   it. The screen should turn **blank/black** — that's the sign it
   worked. Keep holding BOOT about 2 more seconds, then release it.
4. Find the board's serial port: on Mac it's something like
   `/dev/cu.usbmodem83101`; on Windows it's `COM3`, `COM4`, etc. (If
   you're not sure which, unplug the board and see which port
   disappears from `esptool.py --port` autodetect or your OS's device
   list.)
5. Run the matching command below, with `<PORT>` replaced by that port:

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
6. When it finishes, the board restarts on its own running the new
   firmware.

Each release's notes describe what changed in that version.

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


## Support
StajPilot is maintained in spare time. More board support, along with new expansion devices like a footswitch controller and a desktop controller, are in the works.
This project keeps moving forward thanks to the people who choose to support it. Every contribution goes straight into the next update.

<a href='https://ko-fi.com/stajd' target='_blank'>
  <img height='56' style='border:0px;height:56px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Support me on ko-fi.com' />
</a>

*Ko-fi has been around since 2011 as a creator support platform — no account needed, just a couple of clicks.


## LICENSE
This software (including any distributed binaries, the "Software") is licensed under the following terms.
1. **Personal / Non-Commercial Use**
   Personal, non-commercial use — including downloading, installing, and running the Software — is
   permitted freely without prior permission. Connecting the hardware to other devices or
   customizing it for your own personal use is also freely permitted without prior permission.
   (Selling or distributing such customized hardware, however, is subject to Section 3 below.)

2. **Commercial Use**
   Any commercial use, including but not limited to use by a company or organization, redistribution,
   or incorporation into a commercial product, should be discussed with the author in advance.

3. **Prohibition on Hardware Productization **
   Regardless of whether the activity is otherwise commercial, the following acts require the
   author's prior consent:

   a. Uploading the Software (including the distributed binary) onto hardware and selling,
      distributing, or otherwise commercializing that hardware as a finished or semi-finished
      product.

   b. Combining hardware running the Software with any additional device (including but not
      limited to enclosures, circuits, sensors, or accessories) into a single product, and selling,
      distributing, or otherwise commercializing that combined product.

   c. Performing, outsourcing, or commissioning (including OEM/ODM arrangements) any of the acts
      described in (a) or (b) above.

   The above acts are considered "commercial use" under this license and should be discussed with
   the author in advance.

## Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED. UNDER NO
CIRCUMSTANCES SHALL THE AUTHOR BE LIABLE FOR ANY CLAIM, DAMAGES (INCLUDING BUT NOT LIMITED TO
DIRECT, INDIRECT, OR INCIDENTAL DAMAGES), OR OTHER LIABILITY ARISING FROM THE USE, INSTALLATION,
OR MODIFICATION OF THE SOFTWARE, OR FROM THE MANUFACTURE OR USE OF ANY HARDWARE INCORPORATING IT.


## Contact

Please open a GitHub Issue, and we'll proceed with further discussion from there.
