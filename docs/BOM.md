# Bill of materials (pre-funding)

**Status: nothing bought.** Purchases are on hold (D-018). Phase 0a is built on a laptop with simulators. The glasses choice is still open (D-019).

Cap: $1,000 before funding. Telescope gear is borrowed from the Purdue Astronomy Club. Prices were checked in November 2026 unless marked "budget"; confirm each one at checkout and record what you actually paid.

## v0 kit (recommended option A, staged VITURE)

| Item | Price | Status |
|---|---|---|
| Raspberry Pi 5, 8 GB | $175.00 | verified |
| Raspberry Pi Active Cooler | ~$5 | secondary source |
| Raspberry Pi SSD Kit 256 GB (M.2 HAT+ and SSD) | ~$65 budget | estimate |
| Official 27 W USB-C power supply (bench use) | $12.95 | verified |
| VITURE HDMI to USB-C Adapter Cable | $29.00 | verified |
| DeskPi KL-P24 micro-HDMI → HDMI board (optional) | ~$12 budget | price unverified |
| VITURE Pro glasses, certified refurbished | $179 list (seen $129–143) | verified, stock varies |
| 12 V → 5 V/5 A USB-C buck converter | ~$15 budget | price unverified |
| 12 V 12 Ah LiFePO4 battery (153.6 Wh) | $62.99 | verified (sale price) |
| Powered USB hub | ~$35 budget | unverified |
| USB lavalier mic + windscreen | ~$12 budget | unverified |
| Bluetooth one-button remote | ~$10 budget | unverified |
| Inline fuse, keyed DC connectors, strain relief | ~$10 budget | unverified |
| **v0 total** | **≈ $623** | ≈ $377 under cap |

**v1 glasses:** VITURE Beast, $549. v0 and v1 together come to about $1,172, over the cap, so the Beast is the first purchase after funding.

## Why this cable and not a generic adapter

Generic HDMI → USB-C adapters send video only. The glasses' motion sensor data can't get back to the Pi through them, so there's no head tracking. The VITURE cable carries video one way and sensor data (USB 2.0) plus power to the glasses the other way, through its USB-A plug into the Pi.

## Before buying anything

1. Borrow first (Purdue Hackers, Bechtel Innovation Design Center, ECE/AAE labs, Astronomy Club).
2. On any borrowed VITURE unit, check that the glasses show up in `lsusb` and that orientation data streams on the Pi.
3. Buy glasses only from a seller with a return window of 30 days or more.
