# StajPilot — Waveshare ESP32-S3-Touch-LCD-2

[← back to all boards](../README.md)

<a href="https://youtu.be/kLVWh-ry95s" target="_blank">
  <img src="https://img.youtube.com/vi/kLVWh-ry95s/maxresdefault.jpg" alt="StajPilot on the Waveshare ESP32-S3-Touch-LCD-2" width="480">
</a>

▶️ **[Watch on YouTube](https://youtu.be/kLVWh-ry95s)**

Touchscreen MIDI controller for the Kemper Profiler Player, built on a
Waveshare ESP32-S3-Touch-LCD-2 (2" 320×240 touch LCD). The board itself
costs around **$15**, and still gives you full touch control - a low-cost,
low-risk way to try StajPilot.

**🚧 Coming soon — firmware releases for this board aren't published yet.**
This page will be filled in with download links, flashing instructions,
and hardware notes once the first release is out.

Questions in the meantime? DM on [Instagram](https://www.instagram.com/stajpilot) —
answered as soon as seen.

## Features

- Bank/rig navigation — 125 song banks × 5 slots, on-screen buttons or
  bank next/prev, kept in two-way sync with the Kemper
- Live rig display — rig name, amp name, gain (10-step color gradient),
  tempo, and fixed-FX on/off state, all pulled live from the Kemper
- Tuner with two modes: **Normal** (integer cents, standard ±2 in-tune
  threshold) and **Fine-Tune** (real sub-cent precision, finer graduated
  bar, tighter in-tune/near-tune bands)
- Tap tempo, including a long-press numeric keypad to type an exact BPM
  and optionally switch the Kemper onto it directly
- Morph — press the already-active slot again to trigger Morph instead
  of reloading the rig
- Bluetooth MIDI central mode — up to 3 BLE MIDI foot controllers
  connected simultaneously, forwarded to the Kemper over USB, with an
  optional MIDI-only scan filter
- WiFi song-config page for labeling banks/slots by song, same as the
  4.3" board's Song Mode
- Settings: brightness, Song Mode, effect-transition animations, and
  tuner Fine-Tune mode all toggle independently

> **⚠️ Caution / 주의**
>
> **KR:** 캠퍼 플레이어 본체는 버튼 자체에 들어오는 조명과, 그 위에 따로
> 박혀있는 색상 LED(뱅크 식별용)를 함께 사용합니다. 이 둘은 원래 완전히
> 동일하거나 거의 동시에 움직입니다. 보다 동기화 되게 개발하는 것이
> StajPilot의 기본 개발 방향입니다. 눈에 띄는 지연이 생길 경우 이미
> 캠퍼에 많은 부하가 걸리고 있다는 신호일 수 있습니다. 이는 캠퍼에게
> 너무 많은 요청을 하거나, 미디 기기가 데이터를 빠르게 처리하지 못해서
> 발생하는 현상입니다. 양방향 통신 중에는 폴링(polling)을 피하는 것도
> 일반적으로 권장되는 방식입니다. 심해지면 캠퍼가 빨간 불빛 에러를 내며 프리즈될 수
> 있습니다. StajPilot 사용 중 눈에 띄는 지연을 발견하시면 반드시
> 알려주세요.
>
> **EN:** The Kemper Player unit itself has two lights per rig button:
> the light built into the button, and a separate color LED above it
> (used for bank identification). These two normally move completely
> identically, or nearly simultaneously — keeping that sync as tight
> as possible is a core StajPilot development goal. If a
> noticeable delay shows up, it can be a sign the Kemper is already
> under heavy load. This usually happens when too many requests are
> being sent to the Kemper, or when the MIDI device isn't processing
> data fast enough. Avoiding polling during two-way communication is
> also generally recommended practice. In severe cases, the Kemper can throw a
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

## Hardware

- Waveshare ESP32-S3-Touch-LCD-2
- ESP32-S3, native USB + native Bluetooth (no companion chip needed)
- 2" 320×240 touch LCD
- The back of the board is exposed - for now, a simple M2-standoff
  spacer and cover, 3D-printed in plastic, is enough to close it up.
  Drawings for a laser-cut acrylic cover are planned and will be
  provided soon.

  <img src="images/board-back-dimensions.png" alt="Board back and mounting hole dimensions" width="420">
