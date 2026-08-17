# StajPilot

Touchscreen MIDI controller for the Kemper Profiler Player. Bank/rig navigation,
tap tempo, tuner, per-slot effect status with live icon reveal
animations, amp art, and a WiFi-based song/setlist config page you run
from your phone.

This repository hosts **compiled firmware releases and documentation
only**. Source is maintained in a private repository. Each supported
board has its own folder with hardware details, firmware releases, and
setup instructions - features and firmware are independent per board.

## Why "stajPilot"

"Staj" is a stylized spelling of "Stage" - same pronunciation, just
written differently. "Pilot" is about what you're actually doing with
it: sitting in front of the touchscreen, actively flying your rig
through a set in real time - not just a passive display.

## Supported hardware

| Board | Screen | Status |
|---|---|---|
| [Guition JC4880P443C_I_W (rev1.3)](guition-p4(rev1.3)-4.3/) | 4.3" 800×480 | **Current - releases available** |
| Waveshare ESP32-S3-Touch-LCD-4.3C | 4.3" 800×480 | In progress - being re-stabilized (this was the project's original board) |
| Waveshare ESP32-S3-Touch-LCD-2 | 2" 240×320 | In progress - currently being tested |

Pick your board above for firmware, flashing instructions, and full
feature details for that specific device.

## MIDI communication with the Kemper

StajPilot runs in the Kemper's bi-directional mode and relies as much as
possible on the data the Kemper streams automatically, rather than
polling for it - Kemper's own guidance recommends avoiding heavy request
traffic while in bi-directional mode. Some detailed data isn't part of
that auto-stream and still needs a direct request; those requests are
kept optional where possible, so users can choose to skip them. The
whole MIDI layer is built around stability and keeping load on the
Kemper's own system low - the streamed data itself stays real-time,
this is about not piling extra requests on top of it.

## License

<!-- fill in: MIT / no reuse / etc. -->

## Issues / feedback

<!-- fill in once the repo exists, e.g. link to Issues tab -->
