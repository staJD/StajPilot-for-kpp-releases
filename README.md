# StajPilot
<a href="https://www.youtube.com/watch?v=ZbDjtYzA_fI" target="_blank">
  <img src="https://img.youtube.com/vi/ZbDjtYzA_fI/maxresdefault.jpg" alt="StajPilot on the Guition JC4880P4" width="480">
</a>

▶️ **[Watch on YouTube — Guition JC4880P4 (4.3")](https://www.youtube.com/watch?v=ZbDjtYzA_fI)**

<a href="https://youtu.be/kLVWh-ry95s" target="_blank">
  <img src="https://img.youtube.com/vi/kLVWh-ry95s/maxresdefault.jpg" alt="StajPilot on the Waveshare ESP32-S3-Touch-LCD-2" width="480">
</a>

▶️ **[Watch on YouTube — Waveshare ESP32-S3-Touch-LCD-2 (2")](https://youtu.be/kLVWh-ry95s)**

Touchscreen MIDI controller for the Kemper Profiler Player. Bank/rig
navigation, tap tempo, tuner, per-slot effect status with live icon
reveal animations, amp art, and **Song Mode** - StajPilot's own addition
for labeling the Player's native bank/slot structure by song (the Player
has no song concept of its own, just banks/slots), configured from a
WiFi page you access from your phone or computer.

This repository hosts **compiled firmware releases and documentation
only**. Source is maintained in a private repository. Each supported
board has its own folder with hardware details, firmware releases, and
setup instructions - features and firmware are independent per board.

Questions? DM on [Instagram](https://www.instagram.com/stajpilot) — answered as soon as seen.


## Why "stajPilot"

"Staj" is a stylized spelling of "Stage" - same pronunciation, just
written differently. "Pilot" is about what you're actually doing with
it: sitting in front of the touchscreen, actively flying your rig
through a set in real time - not just a passive display.

## Supported hardware

| Board | Screen | Status |
|---|---|---|
| [Guition JC4880P443C_I_W_Y (rev1.3)](guition-p4-rev1.3-4.3/) | 4.3" 800×480 | **Current - releases available** (high-performance ESP32-P4 chipset) |
| Waveshare ESP32-S3-Touch-LCD-4.3C | 4.3" 800×480 | In progress - being re-stabilized (this was the project's original board) |
| [Waveshare ESP32-S3-Touch-LCD-2](WS-S3-2/) | 2" 240×320 | **Coming soon** (compact touch version) |
| Guition JC3248W535C | 3.5" | In progress (a middle-ground size between compact and full) |

Pick your board above for firmware, flashing instructions, and full
feature details for that specific device.

> ⚠️ **Buying a Guition board?** Confirm with the seller that it's
> **P4rev1.3** before you order, and check the model name: **`_Y`** at
> the end means the case is included, no `_Y` means bare board only —
> see the [board page](guition-p4-rev1.3-4.3/) for details.

## Installing

**Easiest way: flash straight from your browser** — no software to
install, just Chrome or Edge on a desktop computer:

### 👉 [stajd.github.io](https://stajd.github.io/)

Plug your board in over USB, pick it from the list on that page, and
follow the on-page instructions.

Prefer to flash manually with `esptool` instead? Each board's folder
above also links to its Releases page with the raw `.bin` files and
manual flashing commands.

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

## Support StajPilot

Keeping up with Kemper updates, building new boards, adding fun features —
StajPilot needs continued work to keep growing. Support is what drives that
forward. I'll keep taking on creative projects that musicians actually need.

### ☕ Support on Ko-fi

<a href='https://ko-fi.com/stajd' target='_blank'>
  <img height='56' style='border:0px;height:56px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Support me on ko-fi.com' />
</a>
<a href="https://ko-fi.com/stajd" target="_blank" style="display:inline-block;background:#E8973A;color:#1A1408;font-weight:700;padding:12px 28px;border-radius:10px;text-decoration:none;vertical-align:middle;">☕ Support on Ko-fi</a>

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
