# Gastown Capstone — "Conference Talk Bingo"

**Goal:** in 5 minutes on stage, show Gastown taking a one-paragraph spec → a working, deployable Conference Talk Bingo web app. Audience scans a QR and plays live for the rest of the talk.

**Why this works as a capstone:**
- It's the "dark factory" version of everything in the prior 50 min — you stop being the controller; the platform handles the orchestration
- It produces an artifact the audience uses *during* the same session — proof that fully-delegated agentic build is real
- Failure is graceful: even a half-finished build with placeholder cards still demos the concept

---

## Pre-stage prep (the night before)

- [ ] Account logged into kilo.ai / Gastown on the demo machine
- [ ] Tethered hotspot ready as wifi backup
- [ ] One full clean run of the spec below, end-to-end, screen-recorded as a 2-min fallback clip embedded in the deck
- [ ] Pre-deployed copy of the finished app at a stable URL (e.g. `bingo.thinkiac.ai` or a Vercel preview) — QR code on the capstone slide points here, NOT to the live Gastown build
- [ ] Bingo word list finalized (see below) — embedded in the spec
- [ ] Browser tabs pre-arranged: (1) Gastown, (2) deployed Bingo app, (3) deck

## On-stage flow (5 min)

| Min | Action | Talking point |
|---|---|---|
| 0:00 | Switch to Gastown tab, paste spec, hit Send | "I'm going to give Gastown the spec. While it's working, look at what's happening." |
| 0:30 | Narrate over the live build: file tree growing, agents working in parallel | "It's spinning up agents I never named. Planning, scaffolding, writing tests, deploying." |
| 1:30 | Switch to the *pre-deployed* Bingo app on the second tab | "Here's the same spec from a clean run last night. QR's on screen — pull it up, play along." |
| 2:00 | Audience plays; you narrate the comparison to earlier demos | "Earlier I was the controller. Here I'm the customer. The harness IS the team now." |
| 3:30 | Switch back to Gastown — show progress, point at the diff/PR view | "By the way, this one's still building. Same prompt, fresh run. This is what L5 looks like." |
| 4:30 | Wrap with the tradeoff slide | "Fast, but you have less idea what's in there. The harness ate the engineering." |

**Recovery rule:** if Gastown stalls, dies, or produces garbage, pivot immediately to the recorded clip in the deck and keep narrating. Do not try to debug live.

---

## The spec to paste into Gastown

```
Build a single-page web app called "Conference Talk Bingo".

Core features:
- A 5x5 bingo grid populated with conference buzzwords (list below)
- "FREE SPACE" in the center
- Each cell is clickable: click toggles between unmarked / marked (with a visible mark)
- Detect bingo: 5 in a row horizontal, vertical, or diagonal — show a celebration overlay
- A "New Card" button that re-shuffles the words into a new grid
- Each player sees a different shuffle (use a session ID in the URL or localStorage seed)
- Mobile-first responsive — most users will scan a QR and play on their phone

Buzzword list (24 words; pick 24 of these per card, randomly):
agentic, AI-native, MCP, sub-agent, orchestration, harness, vibe coding,
spicy autocomplete, dark factory, worktree, sandbox, CLAUDE.md,
hooks, slash command, prompt engineering, context window, RAG,
embedding, fine-tune, evals, hallucination, token, latency, cost.

Style:
- Dark mode by default, terminal/monospace aesthetic
- Catppuccin-ish palette: deep purple background, lavender + green + peach accents
- JetBrains Mono font
- Marked cells get a green X with a slight glow
- Bingo overlay should be celebratory but not nauseating

Stack:
- Single-file static HTML preferred — no backend required
- Vanilla JS or htmx, no React
- Deployable to any static host

Output: a working, deployable static site. Show me the URL.
```

---

## Buzzword list rationale

The 24 words are a mix of:
- Terms I'll definitely say in the talk (agentic, harness, sub-agent, worktree, sandbox, CLAUDE.md, dark factory, vibe coding, spicy autocomplete)
- Generic AI-conference vocabulary that lands as bingo material (RAG, MCP, evals, hallucination, fine-tune)
- Engineering vocabulary the audience will hear from other speakers (latency, cost, context window, token)

Adjust during dress rehearsal once the deck-final word frequency is known.

---

## Tradeoff talking points (post-demo)

When transitioning back to the closing slides:

- **Speed**: minutes to product. No CLAUDE.md, no agent definitions, no tmux.
- **Loss of legibility**: you didn't write the spec for sub-agents — the platform decided. Less control, less learning.
- **Cost**: Gastown-class systems burn tokens aggressively. Compare $/feature to your own controller pattern.
- **The right tool depends on the task**: a startup MVP vs. a regulated codebase have very different answers.
- **The harness ate the engineering** — and that's both the promise and the warning.
