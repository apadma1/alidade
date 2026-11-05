# Architecture

Status: design only. Nothing below is built yet. Numbers are targets to measure, not results.

## What runs where

```
Telescope gear (mount, cameras, focuser, filter wheel)
        │  USB / serial / Wi-Fi
        ▼
Raspberry Pi 5 (8 GB, NVMe, Raspberry Pi OS 64-bit)
  alidade-gateway  talks to gear over INDI (TCP :7624) and ASCOM Alpaca (HTTP :11111)
  alidade-state    one shared, time-stamped record of everything (WebSocket :8765)
  alidade-guard    safety checker; the only path to hardware (see SAFETY.md)
  alidade-solve    works out where the scope points (ASTAP, cedar-solve)
  alidade-stack    live-stacks frames (Siril or OpenLiveStacker, decision D-010)
  alidade-voice    wake word → offline speech-to-text → command; Claude API only when online
  alidade-hud      draws the red display (C++, OpenGL ES 3.1, KMS/DRM, no desktop)
  alidade-web      audience view for a TV/browser + settings page
        │ micro-HDMI → powered HDMI→USB-C (DP Alt) adapter
        ▼
Viture Beast / XREAL One Pro glasses  (IMU head orientation comes back over USB)
```

## How a command flows

```
"go to M51" → wake word → speech-to-text → TypedIntent (JSON)
            → alidade-guard approves or rejects → device adapter → mount
```

The language model can only propose a `TypedIntent`. It never sends anything to hardware.

```json
{
  "intent": "slew_to_target",
  "target": {"catalog": "M", "id": "51", "ra_deg": 202.4696, "dec_deg": 47.1952},
  "source": "grammar",
  "utterance": "go to M fifty one",
  "confidence": 0.93,
  "ts": "2026-11-02T03:14:07.512Z"
}
```

## State bus topics

| Topic | Contents |
|---|---|
| `device.mount` | RA/Dec, Alt/Az, slewing, tracking, pier side, parked |
| `device.camera` | exposure progress, gain, sensor temperature |
| `solve.result` | RA/Dec, rotation, arcsec/pixel, solve time, ok |
| `stack.frame` | frame count, total integration, HFR, preview image URL |
| `guide.stats` | RMS RA/Dec/total, star lost |
| `focus.metric` | HFR, FWHM, focuser position |

## Session states

`IDLE → SLEWING → SOLVING → CENTERING (≤ 3 tries) → TRACKING → SEQUENCING ↔ FOCUSING → IDLE`

Faults (`GUIDE_LOST`, `SOLVE_FAIL`, `CLOUDS`, `DEVICE_DISCONNECT`) each get a HUD card, a spoken message, and a recovery action.

## Display

- 1920×1080 @ 60 Hz to the glasses.
- Two layers: **head-locked** (status panel, alerts, stack thumbnail) and **sky-locked** (labels on stars and objects, placed using the glasses IMU + location + time + a one-star calibration).
- Sky labels need only rotation tracking (3DoF), because stars are effectively infinitely far away.
- Night mode: everything is rendered red only. No green or blue pixels, ever.
- Target: head turn → display update in under 20 ms.

## Latency budget: voice → mount starts moving

| Stage | Offline (ms) | Online via Claude (ms) |
|---|---|---|
| Detect end of speech | 300 | 300 |
| Speech-to-text | 400–900 | 400–900 |
| Pick the command | 10 | 1,000–2,500 |
| Safety check | 5 | 5 |
| Command reaches mount | 50–200 | 50–200 |
| **Total** | **~0.8–1.4 s** | **~1.8–3.9 s** |

## Open questions blocking the design

- E-01: do the glasses show video from the Pi through the adapter?
- E-02: can the Pi read the glasses' IMU, and how much does it drift?
- D-010: Siril vs. OpenLiveStacker. D-011: license.
- `alidade-web` on port 80 needs extra Linux permissions; may use :8080 instead.
