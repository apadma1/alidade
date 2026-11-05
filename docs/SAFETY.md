# Safety rules (`alidade-guard`)

Status: rules defined, not yet implemented.

## The one law

Voice and AI produce a **TypedIntent**. Only `alidade-guard` can turn it into a hardware command. There is no other path. The guard is plain, deterministic code: the same input always gives the same answer, and no language model is involved.

## Rules

| Rule | Check | Result |
|---|---|---|
| Sun avoidance | Target or slew path within 30° of the Sun and no solar filter flagged | Reject |
| Horizon limit | Target altitude below the minimum (default 15°) | Reject, and say when it rises |
| Meridian flip | Target needs a pier flip mid-sequence | Plan the flip, or reject with a reason |
| Mount limits | Driver reports a cable-wrap or hard limit | Reject |
| Rate limit | More than one slew per 5 s | Queue or ask to confirm |
| Low confidence | Speech confidence below 0.80 on any motion command | Ask "Slew to M51?" before moving |
| Abort | "Stop", "abort", or the button | Skips every queue; mount stops in < 300 ms |

## How we prove it

- A test suite of 50 deliberately unsafe commands (Sun, below horizon, flips, nonsense). Required result: 100% rejected.
- Abort timing measured on the INDI simulator and on real hardware.
- Any change to these rules needs a decision-log entry and must keep the suite at 100%.
