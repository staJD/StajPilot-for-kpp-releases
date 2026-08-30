# StajPilot — Waveshare ESP32-S3-Touch-LCD-2

[← back to all boards](../README.md) · [한국어로 보기](README_kr.md)

<a href="https://youtu.be/kLVWh-ry95s" target="_blank">
  <img src="https://img.youtube.com/vi/kLVWh-ry95s/maxresdefault.jpg" alt="StajPilot on the Waveshare ESP32-S3-Touch-LCD-2" width="480">
</a>

▶️ **[Watch on YouTube](https://youtu.be/kLVWh-ry95s)**

Touchscreen MIDI controller for the Kemper Profiler Player, built on a
Waveshare ESP32-S3-Touch-LCD-2 (2" 320×240 touch LCD). The board itself
costs around **$15**, and still gives you full touch control - a low-cost,
low-risk way to try StajPilot.

**The beta is out.** Small doesn't mean less capable - there's actually
a lot packed in behind various gestures.

Questions? DM on [Instagram](https://www.instagram.com/stajpilot) —
answered as soon as seen.

## Features

- Bank/rig navigation — 125 song banks × 5 slots, on-screen buttons or
  bank next/prev, kept in two-way sync with the Kemper
- Live rig display — rig name, amp name, gain (10-step color gradient),
  tempo, and fixed-FX on/off state, all pulled live from the Kemper
- 8 main effect slots shown with their own color/icon per effect type,
  on/off at a glance
- Tuner with two modes: **Normal** (integer cents, standard ±2 in-tune
  threshold) and **Fine-Tune** (real sub-cent precision, finer graduated
  bar, tighter in-tune/near-tune bands), with the screen border lighting
  up green when in tune
- Tap tempo, including a long-press numeric keypad to type an exact BPM
  and optionally switch the Kemper onto it directly
- Morph — press the already-active slot again to trigger Morph instead
  of reloading the rig
- Morph gauge bar — visualizes morph position with a red-to-blue
  gradient (red left, blue right) so you can see it at a glance
- Zoomed display mode — blows up the rig/song name to fill the screen
  so it's readable from a distance
- Bluetooth MIDI central mode — up to 3 BLE MIDI foot controllers
  connected simultaneously, forwarded to the Kemper over USB
- MIDI-priority scanning — the Bluetooth scan surfaces MIDI-capable
  devices first, so you're not digging through unrelated BLE devices
- Bluetooth whitelist — register the specific device(s) you use so
  only those connect, preventing other Bluetooth devices from
  connecting by accident
- Host mode support — the USB-A port can be handed off to a second
  bi-directional device (this needs a power-capable OTG adapter)
- WiFi song-config page for labeling banks/slots by song, same as the
  4.3" board's Song Mode
- Song Mode multi-language support — song names can be shown in
  Korean, accented Latin characters, and more (language support is
  expanding)
- Settings: brightness, Song Mode, effect-transition animations, and
  tuner Fine-Tune mode all toggle independently

> **⚠️ Caution / 주의**
>
> **KR:** 캠퍼 플레이어 본체는 버튼 자체에 들어오는 조명과, 그 위에 따로
> 박혀있는 색상 LED(뱅크 식별용)를 함께 사용합니다. 이 둘은 원래 완전히
> 동일하거나 거의 동시에 움직입니다. 보다 동기화 되게 개발하는 것이
> StajPilot의 기본 개발 방향입니다. 눈에 띄는 지연이 생길 경우 캠퍼에
> 많은 부하가 걸리고 있다는 신호이거나, 연결된 기기에서 신호를 처리하지
> 못하는 병목의 신호일 수 있습니다. 이는 캠퍼에게
> 너무 많은 요청을 하거나, 미디 기기가 데이터를 빠르게 처리하지 못해서
> 발생하는 현상입니다. 그들도 양방향 통신 중에는 폴링(polling)을 피하라고
> 권고하고 있습니다. 심해지면 캠퍼가 빨간 불빛 에러를 내며 프리즈될 수
> 있습니다. StajPilot 사용 중 눈에 띄는 지연을 발견하시면 반드시
> 알려주세요.
>
> **EN:** The Kemper Player unit itself has two lights per rig button:
> the light built into the button, and a separate color LED above it
> (used for bank identification). These two normally move completely
> identically, or nearly simultaneously — keeping that sync as tight
> as possible is a core StajPilot development goal. If a
> noticeable delay shows up, it can be a sign the Kemper is already
> under heavy load, or a sign of a bottleneck where the connected
> device can't keep up processing the signal. This usually happens
> when too many requests are being sent to the Kemper, or when the
> MIDI device isn't processing
> data fast enough. They also recommend avoiding polling during
> two-way communication. In severe cases, the Kemper can throw a
> red-light error and freeze. If you notice a noticeable delay while
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

Pick "Waveshare ESP32-S3 2.0" from the list, then:

1. Press and hold the board's **BOOT** button. While still holding it,
   plug in the USB-C cable to the board's Type-C port, connected to
   your computer. Watch the screen: it should turn **blank/black** —
   that's the sign it worked and the board is in upload mode (if the
   normal display is still showing, it didn't work — try again). Once
   it's black, keep holding BOOT about 2 more seconds, then release it.

   <img src="images/boot-button.png" alt="RST button, Type-C port, and BOOT button locations" width="420">

2. Click **Connect & Flash** on the site.
3. A popup will list every port on your computer — pick the one for
   this board, then click **Connect**.
   - **On Mac**: look for a single entry whose name includes both
     `usbmodem` and `USB JTAG/serial debug unit` together. Ignore
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
6. When you see "Installation complete!", click **Next**, then close
   the tool's menu with the **X** in the corner.
7. Unplug the cable from your computer, then connect it to your Kemper
   as normal — this board only has one USB-C port, so the same cable
   and port used for flashing is also what connects to the Kemper.

### Manual install with esptool

If you'd rather flash it yourself from the command line, grab the
latest release for this board from the [Releases](../../../releases)
page and see that release's notes for the exact `esptool.py` commands
and flash offsets.

## Hardware

- Waveshare ESP32-S3-Touch-LCD-2
- ESP32-S3, native USB + native Bluetooth (no companion chip needed)
- 2" 320×240 touch LCD
- The back of the board is exposed - for now, a simple M2-standoff
  spacer and cover, 3D-printed in plastic, is enough to close it up.
  Drawings for a laser-cut acrylic cover are planned and will be
  provided soon.

  <img src="images/board-back-dimensions.png" alt="Board back and mounting hole dimensions" width="420">
