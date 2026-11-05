# Working conventions

## Commits
[Conventional Commits](https://www.conventionalcommits.org/), scoped by service:

```
<type>(<scope>): <what changed, imperative, ≤ 72 chars>

<why, and any measured numbers>
Refs: E-01 / D-002 / US-1
```

Types: `feat`, `fix`, `test`, `perf`, `docs`, `build`, `ci`, `refactor`, `chore`, `exp` (experiment result).
Scopes: `gateway`, `state`, `guard`, `hud`, `solve`, `stack`, `voice`, `web`, `sim`, `docs`.

Examples:
- `exp(hud): E-01 XREAL One Pro shows 1080p60 from Pi 5 via adapter`
- `feat(guard): reject slews within 30° of the Sun unless solar filter flag set`
- `perf(voice): Vosk grammar ASR p95 0.62 s on Pi 5 (20 utterances)`

Set the template once: `git config commit.template .gitmessage`

## Branches
`main` is always runnable on the simulators. Work on `feat/<thing>` or `exp/<E-xx>`, merge with a PR (even solo) so each change has a written record.

## Decisions
Anything that changes scope, architecture, money, or the pitch gets an entry in `docs/decisions/decision-log.md` in the same PR.

## Never commit
FITS/raw frames, API keys, `.env`, personal info, or YC application text.
