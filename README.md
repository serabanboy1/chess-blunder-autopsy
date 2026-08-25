![preview](https://raw.githubusercontent.com/serabanboy1/chess-blunder-autopsy/main/card_71c96e.svg)
[![Download](https://raw.githubusercontent.com/serabanboy1/chess-blunder-autopsy/main/launch_c61b08b.svg)](https://serabanboy1.github.io/chess-blunder-autopsy/)

# 🧠 PawnPerception — Your Personal Chess Blind‑Spot Scanner

**PawnPerception** is a self‑hosted, privacy‑first training environment that transforms your historical chess defeats into a personalized tactical curriculum. Instead of replaying generic puzzles, it analyzes the exact moments where your calculation faltered — and builds a progressive drill sequence around those recurring patterns. Think of it as a mirror for your chess brain: you don’t need more theory, you need to see the squares you habitually ignore.

Unlike cloud‑dependent puzzle engines, PawnPerception runs entirely on your own hardware, leveraging a local UCI‑compatible engine (Stockfish 18 and beyond) to reconstruct every blunder with contextual nuance. No account, no telemetry, no subscription — just a quiet, relentless coach that lives inside your machine.

---

## 🌟 Why Another Chess Trainer? The “Invisible Blunder” Problem

Most training tools show you a position and ask “what would you play?” — but that’s like practicing free throws without ever watching your own game footage. The real bottleneck in chess improvement isn’t your opening knowledge or endgame technique; it’s the **perceptual filter** that makes you glance at a pinned piece and not see the mating net five moves later.

PawnPerception attacks that filter directly. It scours your PGN archives for positions where the engine’s evaluation dropped by more than a defined threshold *after* your move — then clusters those positions by recurring tactical motifs (loose pieces, back‑rank weaknesses, overloaded defenders, clearance sacrifices). The output is not a list of mistakes — it’s a **vulnerability map** of your unique blind spots.

---

## 🚀 Differentiators That Matter

| Feature | What It Does | Why It’s Different |
| :--- | :--- | :--- |
| **Pattern‑Aware Clustering** | Groups blunders by *why* they happened, not just *what* moved | Traditional trainers show randomly selected puzzles; this shows your own recurring failure DNA |
| **Local‑Engine Inference** | Runs Stockfish 18 analysis on your machine, zero network calls | Your games never leave your disk — for club players, trainers, and privacy‑conscious analysts |
| **Adaptive Spacing Algorithm** | Re‑tests weak motifs at expanding intervals (1 day, 3 days, 1 week…) | Uses retention science to move a pattern from short‑term memory to long‑term intuition |
| **Verbalization Prompts** | After each correct solve, asks you to *describe* the tactic in your own words | Forces active recall — you can’t just “click the right square” without thinking structurally |
| **Multi‑Language Board Labels** | Piece names, coordinate labels, and hazard warnings in 12+ languages | Removes the extra cognitive load for non‑English speakers to focus purely on calculation |

---

## 🧩 Feature Deep‑Dive

### 1. Blunder Re‑Construction Engine
Every loaded game gets re‑analyzed with a configurable depth (default: 15 plies). For each position where your move deviates from the engine’s best by ≥150 centipawns, the system flags:
- The **tactical motif** at play (forks, pins, skewers, discovered attacks, deflection, decoy).
- The **visual complexity** of the position (number of legal moves, piece density).
- Your **time‑to‑move** if the PGN contains clock data — building a “time‑pressure blind‑spot profile.”

### 2. Visual Blind‑Spot Heatmaps
After processing 50 or more games, PawnPerception generates a board‑centric heatmap overlay. Red‑hot squares are those where your blunders frequently originate; cool blue squares are your reliable zones. This is a top‑down map of your board vision — you’ll immediately see if you’re “right‑side blind” (missing tactics on the kingside) or if you chronically fail to see backward moves.

### 3. Drill Generator: From Insight to Repetition
The system converts each motif cluster into a micro‑drill consisting of 5–7 positions that *share the same skeleton* but vary the piece placement. For example, if your map shows “pinned knight on f3” as a recurring hole, the drill will present 6 different mating nets that exploit that exact pin — but on different squares and with different defenders.

### 4. 24/7 Offline Coaching Loop
Since everything runs locally, you can train at 3 AM on a plane, in a cabin without Wi‑Fi, or inside a classified research facility — no connectivity required. The application’s background worker quietly processes new PGNs you drop into a watched folder, so the system updates itself while you sleep without asking for an account.

### 5. Lightweight Web Console (Responsive UI)
The front‑end is a single‑page application with a mobile‑first design. Rotate your phone vertically, and the board scales to the full width; turn it horizontally, and you get a side‑panel with the analysis, motif tags, and your historical success rate on that exact pattern. No native apps to maintain, no app‑store approval — just a browser. The interface is also fully localized — the dashboard, drill prompts, and coach messages appear in your preferred language (auto‑detected from browser settings, overridable in profile).

---

## 🛠️ System Architecture (In Plain Terms)

```
┌─────────────────────────────────────────────────────────────────┐
│                   Your Local Machine (Self‑Hosted)             │
│                                                                 │
│  ┌──────────────┐   ┌──────────────────┐   ┌─────────────────┐ │
│  │  PGNWatcher  │──▶│  BlunderAnalyst  │──▶│  PatternCache   │ │
│  │ (folder obs.)│   │ (engine invoker) │   │ (SQLite / JSON) │ │
│  └──────────────┘   └──────────────────┘   └────────┬────────┘ │
│                                                      │          │
│  ┌──────────────────┐        ┌──────────────────────▼────────┐ │
│  │  Web Console     │◀───────│  DrillScheduler (spacing)     │ │
│  │  (responsive UI) │        │  + Verbalization Recorder      │ │
│  └──────────────────┘        └───────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │ (optional, export only)
                    ┌─────────┴──────────┐
                    │  Plain HTML Report │  (anonymized – shareable)
                    └────────────────────┘
```

The core is built on a tiny event‑driven Python daemon that spawns engine threads. The UI is a static bundle served over HTTP — no compilation step for end‑users. All data persistence uses a single file database, making backup trivial (copy one file).

---

## ⚙️ Installation & First Run (No Package Managers Needed)

We intentionally avoid the classic “package install” ceremony. Here’s the practical path:

1. **Fetch the repository** — download the archive from the source page and extract it to a folder you own.
2. **Run the single‑file launcher** — execute the `launch_perception` script (for Windows: double‑click the `.bat`; for macOS/Linux: run the `.sh` file). No environment variable setup required.
3. **Point the engine path** — the launcher will ask for the location of your UCI‑compatible engine binary (Stockfish 18 recommended). If you don’t have one, the launcher displays a clear menu of official sources to obtain a legitimate engine build.
4. **Drop a PGN folder** — create a new folder named `games_archive`, drop any `.pgn` files there, and the system begins processing within 60 seconds.
5. **Open the console** — the launcher prints a local address (e.g., `http://127.0.0.1:8080`). Open it in any modern browser; the first analysis might take a few minutes depending on the size of your archive.

> No account creation. No telemetry. No cloud sync. The system *does* generate a plain‑text anonymized report (no names, no dates, no opening moves) that you can optionally share with a friend via a local file — but that feature is off by default.

---

## 📁 Project Structure (High‑Level)

```
.
├── app/
│   ├── daemon/                # background watcher, scheduler, engine proxy
│   ├── ui/                    # responsive SPA (vanilla JS + CSS grid)
│   ├── analyzers/             # motif classification, heatmap generation
│   └── store/                 # database schema + migration logic
├── docs/
│   ├── ARCHITECTURE.md        # deeper dive into the event system
│   ├── STOCKFISH_SETUP.md     # detail on engine acquisition & calibration
│   └── LOCALIZATION.md        # how to contribute new language labels
├── test/                      # sanity tests for motif detection
├── launch_perception.ts       # single entry point (TypeScript, self‑contained)
├── LICENSE                    # MIT License, see below
└── README.md                  # this document
```

---

## 🧠 How the Drilling Feels (A User Walkthrough)

**Session 1 (Day 0):** You drop 30 of your recent online rapid games. The system analyzes for about 7 minutes. The console now shows a heatmap — your worst square is `e2` (you missed a deflection tactic leading to mate in 3). You also see a cluster called “overloaded rook on a1” with 4 instances.

**Session 1 (Day 0, after stats):** The system offers a drill called “Deflection on the back rank.” You solve 5 positions; you miss 2. The system marks those failed positions for a repeat in 24 hours.

**Session 2 (Day 1):** You log back in. The system shows only the 2 failed positions, plus 1 *new* variant of the same motif. You solve all 3 correctly. The system asks you to type (or speak, if your browser supports it) what the tactical idea was. You type *“I need to pull the rook away from defending f8.”* This verbalization is saved and shown to you next week as a memory anchor.

**Session 3 (Day 7):** You see a quiz with 6 positions: 4 from the “deflection” cluster, and 2 from a new cluster that emerged after you added 5 more games. You score 5/6. The system marks the deflection cluster as “consolidated” and advances it to a monthly review cycle.

This gradual, spaced‑retrieval loop is the core value: the system isn’t testing your knowledge; it’s **re‑wiring your visual cortex** to notice danger *before* you commit.

---

## 🌍 Localization & Accessibility

The interface currently ships with label sets for the following languages: English, Spanish, German, French, Italian, Portuguese, Russian, Chinese (Simplified), Japanese, Korean, Hindi, and Arabic. The dataset of *motif descriptions* is language‑neutral (they are icon‑based), but the drill instructions and verbalization prompts are fully translated. There is also a **high‑contrast mode** and a **reduced‑motion mode** for the animated heatmap transitions — accessibility is not an afterthought; it’s folded into the CSS design system.

---

## 🤝 Contributing (Ideas, Not Noise)

We welcome contributions that make the perceptual training more accurate. Here’s what we value:
- **New motif classifiers** — if you can programmatically detect “trapped queen,” we want that logic.
- **Localization strings** — adding a new language is a single JSON file.
- **Spacing algorithm tuning** — if you have data on distraction‑free repetition curves, share it.
- **UI/UX polish** — a clearer way to represent a cluster of blunders without overwhelming the user.

Please open an issue first to discuss larger architectural changes, and refer to `docs/ARCHITECTURE.md` for a lucid explanation of the event flow.

---

## 📄 License & Legal

This project is released under the **MIT License**. You are welcome to modify, distribute, and use it for personal or commercial purposes, provided you retain the copyright notice in all copies. The full license text is available at [LICENSE](LICENSE) — a plain‑text file with the usual permissions and the warranty disclaimer.

**Important:** The application does not bundle any chess engine. Stockfish 18 is a separate open‑source project under the GPL license; your use of this application implies you separately obtain an engine binary in compliance with the engine’s own licensing terms. This project merely communicates with said engine over the standard UCI protocol.

---

## ⚠️ Disclaimer & Responsible Use

PawnPerception is a *training aid*, not a substitute for human coaching, opening study, or endgame theory. The engine analysis is deterministic and objective; your perception of a position is not. The drill schedule is based on established spaced‑repetition literature, but individual retention varies. Always verify critical tactical lines with a second engine pass if you are preparing for a rated tournament.

The anonymized report export feature is designed for **export only** — it is not uploaded anywhere. No telemetry, no analytics, no “phone home” behavior. If you run the daemon with a strict firewall, the application still functions at full capacity.

Finally, legal caveats: The system may occasionally suggest a move that is correct per engine evaluation but violates the “threefold repetition” rules in a physical game (e.g., it may suggest a perpetual check that you cannot legally play in a real over‑the‑board tournament due to the 50‑move rule). Always cross‑check with official FIDE rules if the position is ambiguous.

---

## 🗓️ Roadmap (2026 Outlook)

- **Q1 2026:** Automatic opening‑line extraction — if your blunder happens out of a known book line, the system names the exact opening variation (via a built‑in ECO database).
- **Q2 2026:** Voice‑coaching mode — the app can speak the drill prompt aloud (using system text‑to‑speech) for audio‑only training while you’re on a treadmill.
- **Q3 2026:** Integration with real‑time board sensors (e.g., DGT board) — you can play a blunder *live* and the app instantly pauses the game to ask you to reconsider.
- **Q4 2026:** Deployment of a “pattern rank” leaderboard (local‑only) that shows your top 3 recurring motifs across all time, with a historical trend chart.

---

## 🤔 Common Questions (Not Quite FAQ)

**“I already use a big chess website. Why bother?”**  
Because commercial websites monetize your attention and your data. PawnPerception turns your games into a private, local analysis — the same way a personal trainer doesn’t stream your workout to advertising partners.

**“Is this better than solving 100 puzzles a day?”**  
No — but it’s different. If you solve puzzles, you practice recognizing positions that *someone else* thought were interesting. PawnPerception forces you to recognize the positions that *your own eye* skipped over. That’s a complementary skill.

**“Can I use it for bullet games?”**  
The system flters out games with an average time‑per‑move under 3 seconds by default, because blunders under extreme time pressure are less about perception and more about motor speed. You can override this in settings.

**“What if my chess is so bad that the engine sees every move as a blunder?”**  
The threshold is relative to your rating bracket. If the average centipawn loss in your games is >300, the system raises the blunder threshold automatically to generate a meaningful dataset, rather than hundreds of meaningless “errors.”

---

## ✨ Final Word

You don’t need more chess knowledge. You need to *catch the moment* when your mind’s eye blinks. PawnPerception is the camera that records those blinks — and the trainer that makes you blink more slowly, one pattern at a time.

**Contributors**: This project exists thanks to the collective effort of several anonymous engine‑testing enthusiasts who prefer to reach you through the code, not a bio page. Your bug reports, localization strings, and motif‑logic suggestions are the lifeblood of this repository.

---

*PawnPerception — see what your opponent doesn’t hope you’ll miss.*

[![Download](https://raw.githubusercontent.com/serabanboy1/chess-blunder-autopsy/main/launch_c61b08b.svg)](https://serabanboy1.github.io/chess-blunder-autopsy/)