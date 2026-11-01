# CIV_4_Bot

**You set the objectives. Your civilization makes the moves.**

An autonomous, goal-driven AI player for *Sid Meier's Civilization IV: Beyond the Sword*. You define what your civilization should achieve; the bot decides the strategy, tasks, and actions to get there.

> **Status:** Early development / planning. Nothing is playable yet. See the [Roadmap](#roadmap).

---

## Why

Civ IV demands constant micromanagement of cities, units, and diplomacy. CIV_4_Bot makes you the strategist instead. Give it objectives, such as "Cultural victory by turn 300" or "Stay at peace and out-tech everyone," and it runs the empire, adapts to changing conditions, and competes against the built-in AI.

## How it works

Civ IV embeds **Python 2.4** and exposes the game through the `Cy*` API (`CyGame`, `CyPlayer`, `CyCity`, `CyUnit`, `CyMap`). To avoid fragile screen-scraping, the project is split in two:

```
┌──────────────────────┐   JSON state    ┌──────────────────────────┐
│  In-game shim        │ ──────────────▶ │  External brain          │
│  (Python 2.4 mod)    │                 │  (Python 3 desktop app)  │
│  hooks CvEventManager│ ◀────────────── │  objectives → commands   │
└──────────────────────┘    commands     └──────────────────────────┘
```

- **Shim:** hooks turn events (e.g. `onBeginPlayerTurn`), exports game state, and executes returned commands. Transport (socket vs. file exchange) is still to be decided.
- **Brain:** the desktop application. Modern Python 3, free of the 2.4 constraints.

## Architecture

| Layer | Responsibility |
|---|---|
| **Objectives** | Player goals and constraints |
| **Strategy** | Converts objectives into priorities: expansion, tech path, civics, war/peace |
| **Operational** | City governors, worker manager, military manager, diplomacy, espionage |
| **Tactical** | Unit moves, combat odds, pathing |
| **Bridge** | State export and command execution |

## Game data

Unit, building, tech, and civic data is **parsed from the game's own XML** (`Assets/XML/...`), not hard-coded. This keeps the bot correct across mods and patch versions. Hand-written reference notes in `docs/` are for human sanity checks only and are not a source of truth.

## Roadmap

- [ ] **1. Bridge:** read-only per-turn state dump (cities, units, techs, diplomacy)
- [ ] **2. End-turn bot:** issues commands and ends turns without crashing
- [ ] **3. Expansion and builds:** settler/worker logic, scored per-city build queues
- [ ] **4. Tech and civic planner** driven by the active objective
- [ ] **5. Military and diplomacy** (hardest; deliberately last)
- [ ] **6. Objective UI and decision log** so you can see *why* the bot did something

## Requirements

- Civilization IV: Beyond the Sword (patched)
- Python 3.10+ (desktop app)
- Python 2.4 is bundled with the game (shim)

## Repository layout (planned)

```
CIV_4_Bot/
├── shim/        # Python 2.4 in-game mod
├── brain/       # Python 3 planner, strategy, managers
├── ui/          # Desktop app and objective editor
├── data/        # XML parsers and cached game data
├── docs/        # Design notes and reference material
└── tests/
```

## Contributing

The project is at the design stage, so issues and design discussion are the most useful contributions right now. Setup and contribution guidelines will follow once the bridge prototype exists.

## License

TBD
