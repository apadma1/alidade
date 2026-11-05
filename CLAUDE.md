# CLAUDE.md: rules for AI coding sessions on Alidade

Read this before writing or changing code in this repo.

## Project in one line
Raspberry Pi 5 at the telescope; controls gear over INDI/Alpaca; red HUD in tethered glasses; voice control; works offline. Details: `docs/ARCHITECTURE.md`.

## Current stage
Pre-prototype. Check `CHANGELOG.md` and the latest `docs/devlog/` entry for what exists. Never describe planned features as built.

## Hard rules
1. **Safety:** no code path may send a command to hardware except through `alidade-guard`. Language-model output is only ever a `TypedIntent`. See `docs/SAFETY.md`.
2. **Offline first:** mount, camera, solve, stack, HUD, and core voice commands must work with no internet. Claude API is optional, for explanations and troubleshooting.
3. **Night-safe display:** HUD output is red only. No asset or shader may produce non-zero green or blue.
4. **No stubs:** do not add empty files, placeholder functions, or `TODO` bodies. A file appears when it works and has a test.
5. **Measure, don't guess:** latency and performance claims need a test or benchmark in `tests/`.

## Layout (grows as code lands)
- Python 3.12 package in `src/alidade/` (`gateway`, `state`, `guard`, `solve`, `stack`, `voice`, `web`)
- C++20 compositor in `src/compositor/`
- Tests in `tests/`, helper scripts in `scripts/`

## Working style
- Small PRs, one topic each, using the commit format in `CONTRIBUTING.md`.
- Scope- or architecture-changing work adds an entry to `docs/decisions/decision-log.md`.
- Test against INDI simulators (`indi_simulator_telescope`, `indi_simulator_ccd`) and Alpaca OmniSim before real hardware.
- Explain changes in plain language in the PR description; the founder reviews every PR.
