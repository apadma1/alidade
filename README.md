# Alidade

A scope-side Raspberry Pi 5 box that runs a telescope over INDI / ASCOM Alpaca, plate-solves and live-stacks locally, and drives a red-on-black HUD in tethered micro-OLED glasses (Viture Beast / XREAL One Pro). You control it by voice. Core control works with no internet.

## Status

**Pre-prototype.** No working code yet. This repo starts the public build log for the Phase 0 spike (Nov 2026):

- [ ] E-01: Pi 5 micro-HDMI → powered HDMI→USB-C (DP Alt) adapter → glasses shows an image
- [ ] E-02: Read the glasses IMU over USB HID on the Pi; measure drift over 10 min
- [ ] INDI telescope + CCD simulators running on the Pi
- [ ] Offline grammar for 5 intents ("go to M51", "abort", ...)
- [ ] 60 s uncut clip: "go to M51" → simulated mount slews → HUD updates

Progress is logged in [`CHANGELOG.md`](CHANGELOG.md) and [`docs/devlog/`](docs/devlog/). Design decisions live in [`docs/decisions/`](docs/decisions/).

## Planned layout

| Path | Service | Language |
|---|---|---|
| `src/gateway/` | `alidade-gateway`: INDI (:7624) and Alpaca (:11111) adapters | Python 3.12 |
| `src/state/` | `alidade-state`: pub/sub + WebSocket state bus (:8765) | Python 3.12 |
| `src/guard/` | `alidade-guard`: deterministic intent validator (sun, horizon, meridian, rate, confidence, abort) | Python 3.12 |
| `src/compositor/` | `alidade-hud`: OpenGL ES 3.1 on KMS/DRM, red-only night shader, 3DoF sky labels | C++20 |
| `src/solve/` | ASTAP CLI / cedar-solve workers | Python 3.12 |
| `src/voice/` | openWakeWord + Vosk/whisper.cpp grammar; Claude API tool-use when online | Python 3.12 |
| `tests/` | Simulator integration tests, 50-case unsafe-command suite, latency harness | Python |

Folders appear as code lands; nothing here is a stub.

## Safety rule

No language model or voice path commands hardware directly. Every action is a typed intent that `alidade-guard` approves or rejects first.

## License

Not chosen yet (decision D-011: GPL-3.0 vs. Apache-2.0). Until a LICENSE file is added, all rights are reserved.
