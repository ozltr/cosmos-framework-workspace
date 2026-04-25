# Galaxy
Genesis document for a comsos with structure. Read this first to understand the entire system.

## What is Galaxy
AI-augmented repository. One person, one repository, with a clear goal. Can be for one or many, like: research, analysis, books, health, business, code decisions, coding, AI skills. The name Galaxy frames the entire system as a structured cosmos — not a flat file dump.

## Ontology (7 objects)
| Object | Alias | Role |
|---|---|---|
| Repository | Galaxy | the whole space — might be one repository, or can have sub-repos |
| Area | Solar System | top-level area, often root folder (e.g. ai-spec, inbox/, workspace/) |
| Topic | Planet | a container , can live in workspace/ |
| Sub topic | Moon | A sub-topic orbiting a planet (or moon) |
| Project | Operations | structured effort for repo, area, topic, etc |
| Protocol | Laws | codified workflow that defines how a specific operation is executed, can be skills or instructions |
| Task | — | actionable unit within a project |
| Capability | Skill / Agent / Plane | what the system can do; lives in ai-spec |
| Schemas | Constitution | repository level files |

## Philosophy
- **Augmented cognition** — repository is a cold extended memory storage; AI agents are collaborators, not tools
- **Structure over entropy** — everything has a place; no orphan files
- **Permanent record** — decisions logged; work logged; nothing appears or disappears without traceability
- **Solo developer** — one owner; minimal maintenance; highly automated; conventions exist for clarity

## Old Galaxy Architecture
```
galaxy/
├── galaxy.md              ← this file (genesis)
├── CLAUDE.md              ← AI opinionated instruction layer
├── galaxy-hq.md           ← ops tracker: Now / Next / Backlog / Explorations
├── galaxy-index.md        ← repository inventory
├── galaxy-journal.md      ← chronological session log
├── galaxy-decisions/      ← galaxy-level ADRs (structure, conventions, ontology), might be redundant
├── projects/              ← Analysis, wiki (Books, Business Library, Personal Dev, politics), v.0.1 place for everything accept AI plane and galaxy level work
├── ai-plane/              ← agent skills, frameworks, decisions, future-capabilities, v.0.1 ai-spec
├── AI instructions/       ← AI principles, solo developer guidelines, markdown conventions, v.0.1 ai-spec
├── outputs/               ← output dir for automated work
├── inbox/                ← raw content for processing
└── assets/                ← images & media
```

## Old Laws
1. **Never work on main** — always branch with descriptive name
2. **galaxy.md overrides all** — system constitution; CLAUDE.md governs AI behavior within it
3. **State artifacts must stay current** — galaxy-hq.md, galaxy-index.md, galaxy-journal.md
4. **ADRs for structural decisions** — two real alternatives existed → galaxy-level → `galaxy-decisions/`; ai-plane → `ai-plane/decisions/`; project → `Projects/<name>/decisions/`

## Old State Artifacts
| File | Role | How to read |
|---|---|---|
| `galaxy-hq.md` | ops tracker + backlog | lines 1–15 for section index, then offset/limit |
| `galaxy-index.md` | vault inventory | lines 1–15 for section index, then offset/limit |
| `galaxy-journal.md` | session history | tail for recent sessions |
| `CLAUDE.md` | AI instructions | always read in full at session start |

## Old Opinionated ai-spec
Full conventions, writing style, and task workflows in `CLAUDE.md`. But moved to .opencode/{skills/,instructions/} and opencode.json and .agent/. 
