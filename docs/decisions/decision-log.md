---
title: 12 Decision Log
type: note
tags: [alidade, decision]
created: 2026-10-29
updated: 2026-11-02
status: evergreen
related: ["[[05_Technical_Architecture]]", "[[02_Company_Brief]]"]
summary: Permanent ADR-style log of strategic and technical decisions, including corrections made during the 2026-11-02 technical expansion.
---
# 12 · Decision Log

Format: **ID · date · title.** Context → options → decision → consequences → owner → status.

---
### D-001 · 2026-10-29 · Vertical wedge: field astrophotography
- **Context:** choosing between a general multi-monitor-replacement product and a vertical niche.
- **Options:** (a) general virtual-desktop OS for glasses; (b) field astrophotography/EAA wedge.
- **Decision:** (b).
- **Reason:** general virtual desktops face platform risk from XREAL/Viture/Meta/Apple first-party software. Astronomy is a harsh, hands-busy test case with open protocols and a reachable community, and the founder already operates in it.
- **Consequence:** horizontal expansion is gated on wedge retention ([[09_Roadmap_and_Milestones]]).
- **Owner:** Anish · **Status:** accepted

### D-002 · 2026-11-02 · Display path: micro-HDMI + DP-Alt adapter (correction)
- **Context:** the original spec said the Pi 5 streams "directly over USB-C DisplayPort Alt Mode". The Pi 5's USB-C port is power-only. Video comes out of 2× micro-HDMI.
- **Decision:** Pi 5 micro-HDMI → powered HDMI→USB-C (DP Alt) adapter → glasses. Fallback: an Android phone with DP Alt as the display host, with the Pi as the server.
- **Consequence:** +$40–80 BOM, one more cable; must be validated by E-01.
- **Status:** accepted, pending E-01

### D-003 · 2026-11-02 · Tracking scope: 3DoF sky registration only in v0 (correction)
- **Context:** the original vision was "overlay data directly onto physical hardware". That needs 6DoF pose + object registration. Neither a Pi nor 3DoF tethered glasses provide it out of the box.
- **Decision:** v0 registers overlays to the **sky** only, using IMU orientation + site/time + a star calibration. This works because celestial objects are at optical infinity, so head translation doesn't matter. Hardware anchoring (6DoF via Viture Beast camera or XREAL Eye + markers) moves to the expansion phases.
- **Status:** accepted

### D-004 · 2026-11-02 · Offline-first voice; Claude is an enhancement (correction)
- **Context:** dark sites often have no cell/Wi-Fi coverage. The original design made the Claude API the voice controller.
- **Decision:** core intents run locally (openWakeWord + Vosk/whisper.cpp + grammar). Claude handles explanations, open questions, troubleshooting, and summaries when a network exists.
- **Status:** accepted

### D-005 · 2026-11-02 · LLM never commands hardware directly
- **Decision:** all intents pass through `alidade-guard` (sun, horizon, meridian, limits, rate, confidence). Abort bypasses the queue.
- **Status:** accepted

### D-006 · 2026-11-02 · Native compositor for the HUD; Chromium only for audience/config
- **Reason:** latency and IMU late-latch. See [[05_Technical_Architecture#3. Display & rendering]].
- **Status:** proposed; revisit after the Phase 0 spike

### D-007 · 2026-11-02 · "3 laptops" demoted to hypothesis H1 (correction)
- **Context:** unverified; ASIAIR/phone-only setups are common.
- **Decision:** don't use it in pitch copy until ≥ 6/10 interviews support it.
- **Status:** accepted

### D-008 · 2026-11-02 · Budget: no YC credits pre-acceptance (correction)
- **Decision:** the COO tracks real Anthropic API spend against the ≤ $1,000 hardware cap and a separate API cap. "YC API credits" was removed from charters.
- **Status:** accepted

### D-009 · 2026-11-02 · C-suite tags are internal lenses, not team members
- **Decision:** never present them as people in YC materials. Solo founder; co-founder search is active.
- **Status:** accepted

### D-010 · Pending · Live stacking engine: Siril scripting vs. OpenLiveStacker
- **Next step:** 1-day spike on the Pi 5; compare CPU, latency per frame, INDI integration, API surface.
- **Status:** open

### D-011 · Pending · Alidade core license (GPL-3.0 vs. Apache-2.0)
- **Next step:** check GPL interplay with Siril/ASTAP/OLS and the OEM plans.
- **Status:** open

### D-012 · 2026-11-02 · Merge personal Smart Glasses AI Project; Meta becomes a secondary client
- **Context:** the personal project (Oct 2026) planned Meta Ray-Ban Display + Android phone hub. Alidade uses tethered micro-OLED glasses + Pi 5.
- **Decision:** primary display = tethered micro-OLED (resolution/FOV make the live stack readable). Meta Ray-Ban Display is kept as an **optional lightweight client** for the sky overlay and status cards via its Web Apps path (orientation + GPS + Neural Band, no camera needed). The `GlassesAdapter` interface carries over as the client-adapter layer.
- **Consequence:** the old "telescope feed on glasses is a gimmick" verdict applies to Meta-class HUDs only. See [[14_Origin_and_Side_Project_Tracks]].
- **Status:** accepted

### D-013 · 2026-11-02 · Cooking assistant parked
- **Decision:** not in the Alidade pitch or roadmap. Revisit after the Phase 1 gate as a candidate second domain or a personal weekend build.
- **Status:** accepted

### D-014 · 2026-11-02 · Rename: Polaris → Reticle Labs (superseded by D-015)
- **Context:** "Polaris" is used by many existing companies.
- **Decision:** renamed to Reticle Labs.
- **Outcome:** dropped the same day. "Reticle" is already used by a semiconductor manufacturer (a reticle is the photomask in chip lithography).
- **Status:** superseded

### D-015 · 2026-11-02 · Rename: Reticle Labs → Alidade
- **Context:** the name has to fit an AR/smart-glasses platform company, not just the astrophotography wedge, and avoid existing companies.
- **Options considered:** Scotopic, Nightglass, Darkframe (all too night/astro-specific); Sightwire; Fieldsight (used by a CMU project and an app); Waylight (close to WayRay); Upline (taken); Fovea (Fovea Pharmaceuticals; close to Fove); Boresight (ASX drone company); Collimate (Collimate Space); Fiducial (French accounting group).
- **Decision:** **Alidade.** An alidade is the sighting arm on an astrolabe or surveying instrument that lines your eye up with a target, the same thing the platform does with live data and hardware. It covers astronomy (first product) and surveying/field engineering (expansion). A web search found no company using it.
- **Consequences:** all vault notes, service names (`alidade-*`), hostname (`alidade.local`) and tags (`alidade`) updated. Pitch line: *"An alidade is how astronomers lined their eye up with the stars. We do that with data."*
- **Still to do:** USPTO search (classes 9 and 42), domain, GitHub org, social handles, update the YC application text.
- **Owner:** Anish · **Status:** accepted pending trademark check

### D-016 · 2026-11-04 · Repository layout and what goes in it
- **Context:** an external review proposed a full repo tree with empty source files, CI workflows, and a GPL-3.0 LICENSE.
- **Decision:** add files only when they contain real content. Docs (architecture, safety, BOM) go in now. Source folders, `pyproject.toml`, CMake, and CI arrive with the first working code. Python code will live in `src/alidade/` so imports work. No LICENSE until D-011 is decided.
- **Reason:** the repo is public and read by YC/Paxel; empty files would overstate progress. Choosing GPL inside a file layout would skip D-011.
- **Owner:** Anish · **Status:** accepted

### D-017 · 2026-11-06 · Shutdowns live in the vault Daily Note, not in a chat
- **Context:** we needed to decide whether end-of-day reflection ("shutdown") happens in the HQ chat or a separate chat.
- **Options:** (a) shutdown in the HQ chat every day; (b) a separate "Shutdown" chat; (c) a 5-minute Daily Note in the vault, with chats only receiving summaries.
- **Decision:** (c).
- **Reasons:**
  - The vault is the single source of truth (00_README rule 1). A chat isn't searchable or linkable from Obsidian.
  - A daily chat message adds friction on tired nights. A 5-line note gets done; a conversation gets skipped.
  - Long chats get slow and lose early context. Daily shutdowns would fill HQ with noise.
- **How it works:**
  1. Each Build or work session ends with the HANDOFF block (P-04). Paste it into today's Daily Note.
  2. Daily Note fields: Done / Blocked / Next / Hours (+ handoff if any).
  3. Post in HQ the same day **only** if the calendar needs to change (a slipped block, a booked interview, a new blocker).
  4. Every Sunday's weekly review pastes the week's Daily Notes into HQ, and HQ rebuilds the next 2 weeks of the calendar.
- **Owner:** Anish · **Status:** accepted

### D-018 · 2026-11-06 · Hardware purchases on hold; laptop + simulators first, borrow before buying
- **Context:** no personal budget for hardware right now. AI credits can't buy parts.
- **Decision:**
  1. Buy nothing for now.
  2. Build Phase 0 software on a laptop against INDI simulators and ASCOM Alpaca OmniSim.
  3. Research a cost-ranked shortlist (P-01) so buying takes a day when money is available.
  4. Ask Purdue Hackers, the Bechtel Innovation Design Center, ECE/AAE labs, and the Astronomy Club to lend a Pi 5 and accessories.
- **Consequences:**
  - E-01 (glasses display path) and E-02 (glasses IMU) can't run until glasses are available. The display path stays the top technical risk.
  - The Dec 6 Phase 0 gate splits in two. **0a:** voice → guard → simulated slew → solve → red HUD on a normal monitor, with voice p95 measured. **0b:** the same loop shown in the glasses, once hardware exists.
  - The YC progress log reports 0a honestly and doesn't imply glasses testing has happened.
- **Revisit when:** money is available, a lender is found, or a YC decision arrives (Dec 11).
- **Owner:** Anish ([COO] lens) · **Status:** accepted

### D-019 · Pending · v0 glasses and display path: staged VITURE vs. Beast-first
- **Context:** 19_Hardware_Shortlist found that VITURE is the only brand with a documented video-plus-sensor path to a Pi (the $29 VITURE cable + XRLinuxDriver). XREAL's sensor path to a Pi is unproven; RayNeo can't track on ARM.
- **Options:** (A) refurbished VITURE Pro now (~$623 total v0), Beast after funding; (C) Beast straight away (~$993, almost no margin); (B) XREAL path; (D) RayNeo display-only.
- **Recommendation from research:** (A). Scores 3.98 vs. 3.39 for (C).
- **Still blocked by D-018:** nothing is bought until money or a lender appears. Run bench tests 2–3 first on any borrowed VITURE unit.
- **Also affects:** the Project instructions and 05 name "Viture Beast / XREAL One Pro" as equal options. Update them if (A) is accepted.
- **Owner:** Anish ([CTO] / [COO] lens) · **Status:** pending

