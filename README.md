# StajPilot

[한국어로 보기](README_kr.md)

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
> 있습니다. StajPilot 사용 중 눈에 띄는 지연을 발견하시면 알려주세요.
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
| [Waveshare ESP32-S3-Touch-LCD-2](ws-s3-2/) | 2" 240×320 | **🎉 Now available - releases open!** (compact touch version) |
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
polling for it - it's best to avoid heavy request traffic while in
bi-directional mode. Some detailed data isn't part of
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
