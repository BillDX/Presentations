# From Spicy Autocomplete to Dark Factory — Build Plan

**Format:** 60-minute demo-led talk (loosely "workshop") for ~75+ attendees
**Source deck:** evolved from `Agentic Coding Workshop/index.html` — preserved separately
**Posture:** demo-driven; laptop optional; take-home starter repo extends the workshop after the room empties

---

## Title arc

The session title *is* the spine of the deck:

| Stage | Stage label | What it looks like |
|---|---|---|
| L0–L1 | **Spicy Autocomplete** | Tab-tab-tab. Copilot. You approve every token. |
| L2 | Vibe Coding / Pair | One agent, one task, you in the loop. Most people. |
| L3 | Delegation | You hand off whole tasks. Sub-agents in parallel. |
| L4 | Orchestration | Agent teams. Pipelines. Worktrees. You're the EM. |
| L5 | **Dark Factory** | Lights-out. Agents running shop. You're the PM, sometimes. |

L0/L1 and L5 are the two new framing endpoints the title promises. The middle three levels are already in the existing deck.

---

## 60-minute slide map

Slide numbers below are targets after restructuring. Items marked **NEW** are not in the source deck.

| # | Section | Slide | Time |
|---|---|---|---|
| 1 | Open | Title — "From Spicy Autocomplete to Dark Factory" + new banner | 1 |
| 2 | Open | The Five Levels (extended with L0/L1 + L5) **NEW columns** | 3 |
| 3 | Open | Single-agent ceiling | 1 |
| 4 | Open | Time, not tokens | 1 |
| 5 | Frame | What we'll do today (demo-led, follow along if you can) | 1 |
| 6 | **Harness** | What is "the harness"? **NEW** | 2 |
| 7 | **Harness** | `CLAUDE.md` — the always-loaded brief **NEW** | 2 |
| 8 | **Harness** | Agent definition files (existing, retitled) | 1 |
| 9 | **Harness** | Slash commands & hooks **NEW** | 2 |
| 10 | **Harness** | Settings & permissions **NEW** | 1 |
| 11 | Sub-agents | Sub-agents — the building block | 1 |
| 12 | Sub-agents | **Demo 1** — reviewer + documenter in parallel | 7 |
| 13 | Sub-agents | Debrief: what just happened | 1 |
| 14 | **Safe envs** | Why isolation matters (blast radius) **NEW** | 1 |
| 15 | **Safe envs** | Git worktrees as the primitive | 1 |
| 16 | **Safe envs** | Sandboxed bash + containers **NEW** | 2 |
| 17 | **Safe envs** | When `--dangerously-skip-permissions` is OK **NEW** | 1 |
| 18 | Controller | The controller pattern | 1 |
| 19 | Controller | **Demo 2** — Plan → Code → Test pipeline | 7 |
| 20 | Teams | Agent teams — peer mesh | 1 |
| 21 | Teams | **Demo 3** — tmux team, brief | 5 |
| 22 | Teams | Sub-agents vs teams (decision rule) | 1 |
| 23 | **Capstone** | Dark Factory: Gastown **NEW** | 1 |
| 24 | **Capstone** | **Demo 4** — Gastown takes a silly project from prompt to product | 5 |
| 25 | Close | Costs & tradeoffs (real $/hour) **NEW** | 2 |
| 26 | Close | Decision tree | 1 |
| 27 | Close | Take-home: starter repo + cheat sheet (QR later) | 1 |
| 28 | Close | Q&A / contact / license | 1 |

**Total slide-time:** ~54 min, leaving ~6 min buffer for transitions, demo recoveries, and Q&A overflow.

**Demo budget:** four demos at ~6 min average. Each has a "fast path" version (3 min) to drop into if running long.

---

## Gastown capstone — proposed silly demo project

The Gastown demo needs to be **legible in 60 seconds**, **visual**, and **silly enough to be memorable** without being so silly the audience can't see the engineering.

### Top picks

1. **"Excuse Generator as a Service"** — a tiny SaaS that generates plausibly-bad excuses for missing meetings, with a leaderboard of upvoted ones. Auth-lite, DB, API, UI, deploy. Hits CRUD + UI + cute domain.
2. **"Cat Standup Bot"** — a Slack-style web app where AI cats do daily standups in character ("yesterday I knocked a glass off the table; today I will sleep on a keyboard; blockers: closed doors"). Visual, funny, easy to grade quality.
3. **"Conference Talk Bingo"** — generate a bingo card of buzzwords ("AI-native", "agentic", "10x") for the current talk; audience can scan a QR and play live during your session. Meta and you can actually *use* it in the room.
4. **"Tiny Town"** — a one-page generative-art town that grows over time with little buildings the user names. Visual, no domain logic to bog the agents down.

**My pick: Conference Talk Bingo** — it doubles as audience engagement, the QR code on the slide gets used twice (downloadable + live game), and "Gastown built the bingo game we're playing" lands as a punchline.

**Fallback: Cat Standup Bot** if Bingo feels too gimmicky in rehearsal.

### Demo prep checklist (Gastown)

- [ ] Confirm kilo.ai/Gastown access works on the conference wifi (test on a tethered hotspot the night before)
- [ ] Pre-warm the prompt — paste the spec into Gastown 5 min before stage so the project tree is partially built
- [ ] Have a recorded screen capture of the same run as a fallback if SaaS dies live
- [ ] Pre-write the spec prompt and save it in the deck speaker notes for paste-ability
- [ ] Decide live-deploy target (Vercel? Replit? a static page?) — or skip deploy and show the running localhost

---

## Take-home starter repo (must-have)

**Repo name:** `spicy-to-dark-factory-starter` (final name TBD)

**What it contains:**

- `README.md` — the workshop after-action: same exercises the deck demoed, with full step-by-step
- A small Python or TypeScript project (~4 files) for the demo target — API + model + utils + a deliberately under-tested function
- `SPEC.md` — feature spec for the controller-pattern exercise
- `.claude/agents/` — pre-written `reviewer.md`, `documenter.md`, `planner.md`, `coder.md`, `tester.md`, `security-reviewer.md`
- `.claude/commands/` — one or two slash commands as examples (`/review`, `/ship`)
- `.claude/settings.json` — example permissions allowlist
- `CLAUDE.md` — example project brief
- `EXERCISES.md` — three self-paced exercises matching the demos
- `BONUS.md` — bug tribunal, TDD red/green pair, full-stack pipeline

**Posture:** the repo IS the workshop. The talk is the trailer.

**To decide:** Python vs TypeScript starter. (Recommend TypeScript — more universally readable for a mixed conference crowd, and Node tooling is friendlier on Macs without setup.)

---

## QR codes (deferred)

Add after the deck is baked. Plan:
- One QR on the take-home slide → starter repo
- One QR on the Gastown capstone slide → live Conference Talk Bingo (if that's the project)
- Optional: one QR on closing slide → slides + recordings
Use a single QR-generation pass at the end so the codes match the final URLs.

---

## Decisions locked

1. **Starter repo language**: **Python** (FastAPI-based mini-app — read-friendly for a mixed conference crowd, batteries-included for "ship a feature" demos)
2. **Gastown project**: **Conference Talk Bingo** — audience scans a QR mid-session and plays live with buzzwords from this talk
3. **Repo hosting**: `github.com/BillDX/spicy-to-dark-factory-starter` (matches the public Presentations repo at github.com/BillDX/Presentations)
4. **Gastown demo script**: yes — write a paste-able prompt + a step-by-step screen-record plan as fallback
5. **Slide aspect / venue**: TBD (assume 16:9, projected; deck is already responsive)

## Still open

- Final Bingo word list — collect during deck rehearsal so it reflects the actual buzzwords used
- Whether to live-deploy the Bingo app from Gastown or just show localhost (recommend: pre-deploy the night before, demo Gastown *building* it live)

---

## Build order (proposed)

1. Restructure the deck slide-by-slide per the map above (this can happen now)
2. Draft the new Harness Engineering, Safe Environments, and Dark Factory slides
3. Iterate on the Gastown demo prompt + rehearse it
4. Build the take-home starter repo
5. Add QR codes once URLs are stable
6. Dry-run end-to-end with timer
