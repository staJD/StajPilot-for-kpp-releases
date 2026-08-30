# StajPilot — Guition JC4880P443C_I_W_Y (rev1.3)

[한국어로 보기](README_kr.md)

<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt="StajPilot" width="480">
</a>

▶️ **[Watch on YouTube](https://www.youtube.com/watch?v=ZbDjtYzA_fI)**

Touchscreen MIDI controller for the Kemper Profiler Player, built on an
ESP32-P4 + ESP32-C6 (Guition JC4880P443C_I_W_Y, 4.3" 800×480 touch LCD).
The P4 chip gives it plenty of headroom for a big 4.3" screen, but the
board itself stays light - it doesn't feel bulky sitting on your
pedalboard or rig. WiFi and Bluetooth are built in too, via a dedicated
companion chip (the ESP32-C6) - no extra modules needed. The board
itself costs around **$30**.

> ⚠️ **Before you buy: confirm the board revision with the seller.**
> This firmware is built and tested specifically against **rev1.3** of
> the Guition JC4880P443C_I_W_Y. Other revisions can differ in pinout and
> components, and are **not guaranteed to work**. Ask the seller to
> confirm it's rev1.3 before you order.
>
> **The model name also tells you whether the case is included**: a
> model ending in **`_Y`** (e.g. `JC4880P443C_I_W_Y`) comes with the
> case/enclosure; without `_Y` (`JC4880P443C_I_W`), it's the bare board
> only. Unless you're sourcing or making your own casing, buy the
> **`_Y`** version — and double-check the case-included option on the
> product listing before you order.

Currently only tested against the Kemper Profiler **Player LV3** - not
verified against the other Profiler models (Stage, PowerHead, Rack).

## Features

- Bank/rig navigation with 5 performance slots + tap tempo + tuner
- Live effect on/off display per slot, with animated reveal icons
  (togglable if you'd rather save CPU)
- Amp art display, loaded from a microSD card (currently being
  stabilized - re-enabled in the next update). **Still in beta** -
  images have to follow a defined format/spec (dimensions, file type,
  etc.), not just any image dropped onto the card. The exact
  requirements are still being finalized and will be documented before
  this is fully re-enabled.
- **Song Mode** - the Kemper Profiler Player has no concept of songs of
  its own, only banks/slots. StajPilot adds a simple Song Mode on top of
  that: a WiFi config page lets you label each bank/slot with a song
  name, sub-label, and amp art file, so the same bank/slot navigation
  you already use also shows song info - no need to touch the device's
  own screen to edit it. This is just per-slot labeling, not a curated,
  reorderable setlist feature.
- Bluetooth MIDI support for wireless foot controllers - tested with 3x
  M-VAVE Chocolate pedals connected simultaneously, up to 5 supported

> **⚠️ Caution / 주의**
>
> **KR:** 캠퍼 플레이어 본체는 버튼 자체에 들어오는 조명과, 그 위에 따로
> 박혀있는 색상 LED(뱅크 식별용)를 함께 사용합니다. 이 둘은 원래 완전히
> 동일하거나 거의 동시에 움직입니다. 보다 동기화 되게 개발하는 것이
> StajPilot의 기본 개발 방향입니다. 눈에 띄는 지연이 생길 경우 캠퍼에
> 많은 부하가 걸리고 있다는 신호이거나, 연결된 기기에서 신호를 처리하지
> 못하는 병목의 신호일 수 있습니다. 이는 캠퍼에게 너무 많은 요청을
> 하거나, 미디 기기가 데이터를 빠르게 처리하지 못해서 발생하는
> 현상입니다. 그들도 양방향 통신 중에는 폴링(polling)을 피하라고
> 권고하고 있습니다. 심해지면 캠퍼가 빨간 불빛 에러를 내며 프리즈될 수
> 있습니다. StajPilot 사용 중 눈에 띄는 지연을 발견하시면 반드시
> 알려주세요.
>
> **EN:** The Kemper Player unit itself has two lights per rig button:
> the light built into the button, and a separate color LED above it
> (used for bank identification). These two normally move completely
> identically, or nearly simultaneously — keeping that sync as tight
> as possible is a core StajPilot development goal. If a noticeable
> delay shows up, it can be a sign the Kemper is already under heavy
> load, or a sign of a bottleneck where the connected device can't
> keep up processing the signal. This usually happens when too many
> requests are being sent to the Kemper, or when the MIDI device isn't
> processing data fast enough. They also recommend avoiding polling
> during two-way communication. In severe cases, the Kemper can throw
> a red-light error and freeze. If you notice a noticeable delay while
> running StajPilot, please report it.

## MIDI communication with the Kemper Player

StajPilot runs in the Kemper Player's bi-directional mode and relies as
much as possible on the data it streams automatically, rather than
polling for it - it's best to avoid heavy request traffic while in
bi-directional mode. Some detailed data isn't part of
that auto-stream and still needs a direct request; those requests are
kept optional where possible, so users can choose to skip them. The
whole MIDI layer is built around stability and keeping load on the
Kemper Player's own system low - the streamed data itself stays
real-time, this is about not piling extra requests on top of it.

## Installing

**Easiest: flash from your browser** — no software to install, works in
Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Pick "Guition JC4880P4" from the list, then:

1. Press and hold the board's **BOOT** button. While still holding it,
   plug in the USB-C cable, connected to the **top** USB-C port (labeled
   PC in the photo below) — not the bottom port, which is for the
   Kemper connection. Watch the screen: it should turn **blank/black**
   — that's the sign it worked and the board is in upload mode (if the
   normal display is still showing, it didn't work — try again). Once
   it's black, keep holding BOOT about 2 more seconds, then release it.

   <img src="images/buttons.jpg" alt="BOOT and RESET buttons" width="260">
   <img src="images/ports.jpg" alt="PC vs Kemper USB-C ports" width="260">

2. Click **Connect & Flash** on the site.
3. A popup will list every port on your computer — pick the one for
   this board, then click **Connect**.
   - **On Mac**: look for a single entry whose name includes both
     `usbmodem` and `USB JTAG/serial debug unit` together, like
     `cu.usbmodem83101 USB JTAG/serial debug unit`. Ignore
     `Bluetooth-Incoming-Port` or `debug-console` — those aren't your
     board.
   - **On Windows**: ports just show as `COM3`, `COM4`, etc. with no
     descriptive name. If you're not sure which one, unplug the board,
     check which port disappears from the list, then plug it back in
     and pick that one.
4. Click **Install**. A dialog will ask if you want to erase the
   device.
   - **First time installing on this board?** Check the box — there's
     nothing saved yet to lose, and it guarantees a clean start.
   - **Already have StajPilot and just updating?** Leave it unchecked
     to keep your saved settings and song list. Then click **Next**.
5. Wait for it to flash — don't unplug the board while it's in
   progress.
6. When you see "Installation complete!", click **Next** (it's the only
   button there). This takes you to a menu with "Install..." and "Logs
   & Console" — that's just the tool's normal menu (it looks the same
   whether you're starting or already finished), so don't click either
   option. Close it with the **X** in the corner instead.
7. Unplug the cable from the top (PC) port, then connect the bottom
   (Kemper) port to your Kemper as normal — that's how you start
   running the new firmware.

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
   the **BOOT** button, then plug the cable back into the **top** USB-C
   port while still holding it (the bottom port is for the Kemper
   connection, not flashing — see photos below). The screen should turn
   **blank/black** — that's the sign it worked. Keep holding BOOT about
   2 more seconds, then release it.

   <img src="images/buttons.jpg" alt="BOOT and RESET buttons" width="260">
   <img src="images/ports.jpg" alt="PC vs Kemper USB-C ports" width="260">

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
6. Unplug the cable from the **top (PC)** port, then connect the
   **bottom (Kemper)** port to your Kemper as normal — that's how you
   start running the new firmware.

Each release's notes describe what changed in that version.

## Hardware

- Guition JC4880P443C_I_W_Y, rev1.3
- ESP32-P4 + ESP32-C6 WiFi/BT companion
- 4.3" 800×480 RGB touch LCD
- microSD card slot for amp art images (optional - the device works
  without one)

## Song Mode config

The Kemper Profiler Player only knows about banks and slots - Song Mode
is StajPilot's own addition, letting you label that same bank/slot
structure by song instead. Turn on WiFi config mode from the device's
Settings screen, connect your phone or computer to the `StajPilot` WiFi
network it broadcasts (default password `12345678`, changeable from the
web config page), then open `192.168.4.1` in a web browser - a
computer's keyboard makes it a lot faster if you're entering a lot of
songs at once. From there you can set
song names, per-slot labels, and which amp art file to show for each
bank - your existing bank/slot navigation then also shows that song
info. There's no separate, reorderable setlist - it's just a label on
each bank/slot.


## Support StajPilot

Keeping up with Kemper updates, building new boards, adding fun features —
StajPilot needs continued work to keep growing. Support is what drives that
forward. I'll keep taking on creative projects that musicians actually need.

### ☕ Support on Ko-fi

<a href='https://ko-fi.com/stajd' target='_blank'>
  <img height='56' style='border:0px;height:56px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Support me on ko-fi.com' />
</a>

The name comes from "coffee" — the idea of buying someone a coffee as a
small, casual way to say thanks. It's a well-known creator-support platform
(since 2011), no account needed, just a card or PayPal, and completely
separate from installing/using StajPilot.


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
For quick questions, DM on [Instagram](https://www.instagram.com/stajpilot) — answered as soon as seen.
