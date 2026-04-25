# Cosmos Spec v0.2.1
*Genesis document for the second iteration of the galaxy system.*
*Derived from grilling sessions — 2026-04-25*

---

## What This Document Is

This is the architecture specification for the Cosmos — a structured system of AI-augmented operating environments. It operates at two levels:

**Level 1 — Cosmos Architecture** (resolved): The stable, high-altitude decisions about structure, governance, and relationships between galaxies. These are unlikely to change. They are the constitution of the system.

**Level 2 — Open Threads** (to be grilled): Topics acknowledged but not yet decided. Listed explicitly so future sessions have a clear agenda.

---

## Level 1 — Cosmos Architecture

### 1. Core Concepts

**Cosmos** — The full system. Universe + all galaxies + all agents + all protocols governing how they relate.

**Universe** — A meta-galaxy. The governance layer above all galaxies. Same structural patterns as a galaxy, but its planets are other galaxies. It is the authority on IAM, capabilities, and universal skills.

**Galaxy** — A scoped operating environment. One git repository, one clear purpose, one owner. Not a file dump. Not just a knowledge store. A place where work happens, agents operate, and state is tracked. The git repository is the persistence layer.

**Agent** — The only actor in the system. Agents live inside galaxies. They read, write, execute, and propose — but they never grant their own access, never modify IAM profiles, and never self-promote skills to an external source without operator approval. Planets are stateful storage. Agents are what make planets change.

**Operator** — A human with a defined role in the system. Operators own and govern galaxies. They resolve conflicts, approve changes, and are the only entities that can grant access or modify IAM.

---

### 2. Universe

The Universe is the meta-galaxy. It governs all other galaxies. It follows the same structural patterns as any galaxy, but its domain is the Cosmos itself — capabilities, IAM, universal skills, and the staging layer between itself and each galaxy it governs.

The Universe is the only entity in the Cosmos with full awareness of all galaxies and their relationships. No galaxy shares this awareness.

**Universe repo structure:**

```
universe/
├── CONTEXT-MAP.md           ← agent routing table
├── galaxy/                  ← universe control plane
│   ├── identity.md          ← universe identity and purpose
│   ├── access.md            ← inter-galaxy access grants (universe authority)
│   ├── agent-spec/          ← universe agent runtime
│   │   ├── conventions.md
│   │   ├── ethics.md
│   │   └── skill-index.yaml
│   ├── hq.md                ← Now / Next / Backlog / Explorations
│   ├── index.md             ← universe inventory
│   └── CONTEXT.md           ← scope routing for galaxy/ dir
├── capabilities.yaml        ← single source of truth: all galaxies, all capability flags
├── skills/                  ← universal skills library
├── member-galaxies/         ← one directory per galaxy: IAM profile, access grants, conflict staging
│   ├── knowledge-bank/
│   ├── tio/
│   └── <galaxy>/
├── inbox/                   ← skill promotion proposals, protocol proposals
└── outputs/                 ← bootstrap outputs, migration specs, generated files
```

**capabilities.yaml** is the single source of truth for what every galaxy in the Cosmos is tagged with. The Universe parses it to determine what to push to each galaxy. Capability lifecycle — add, modify, deprecate — is managed by a local Universe skill and requires code review before propagation.

```yaml
# universe/capabilities.yaml
galaxies:
  knowledge-bank:
    owner: <operator>
    capabilities: [knowledge-store, inter-galactic-source]
  tio:
    owner: <operator>
    capabilities: [software-development, knowledge-store]
  financial:
    owner: <operator>
    capabilities: [knowledge-store, operational]
  personal-ops:
    owner: <operator>
    capabilities: [knowledge-store, software-development]
```

---

### 3. Galaxy

A galaxy is a scoped operating environment. One git repository, one clear purpose, one owner. Not a file dump. Not just a knowledge store. A place where work happens, agents operate, and state is tracked. The git repository is the persistence layer.

**A galaxy has no fixed purpose.** It is tagged with capability flags that describe what it does today. Those flags can expand as the galaxy grows — no migration required.

**Galaxy structure:**

```
<galaxy>/
├── CONTEXT-MAP.md           ← root-level agent routing table, loaded at cold start
├── galaxy/                  ← control plane: galaxy-level ops and external push landing zone
│   ├── identity.md          ← name, purpose, owner
│   ├── access.md            ← which external planets this galaxy may access (read-only by agent)
│   ├── agent-spec/          ← agent runtime: pushed from external source, projected to providers
│   │   ├── conventions.md
│   │   ├── ethics.md
│   │   └── skill-index.yaml ← capability router: names, triggers, sources
│   ├── skills/              ← local galaxy skills
│   ├── hq.md                ← Now / Next / Backlog / Explorations
│   ├── index.md             ← galaxy inventory (see OT-9: may be superseded by CONTEXT.md pattern)
│   └── CONTEXT.md           ← scope routing for galaxy/ control plane
├── inbox/                   ← raw content for processing
├── outputs/                 ← structured outputs from agent work
└── <planet-dirs>/           ← domain-named directories, one per planet
    └── CONTEXT.md           ← planet-level scope routing, file index, agent guidance
```

**The `galaxy/` directory is the control plane.** It is simultaneously a meta-planet (stateful, with its own `CONTEXT.md`) and the landing zone for updates pushed from external sources. Agent runtime, skills, identity, and access grants all live here. External sources push into `galaxy/agent-spec/` — a universe skill then projects this into provider-specific formats (`.opencode/`, `.claude/`, etc.) for the active coding agent.

**The CONTEXT pattern:**

- `CONTEXT-MAP.md` at root — the agent's first routing file after cold start scope question. Maps session scopes to directories and files to load.
- `CONTEXT.md` inside each planet directory — describes what the planet contains, what files live where, and how an agent should navigate it. Reduces the need for directory exploration and file listing at the cost of maintenance.

---

### 4. Operators

Three human roles. Strictly ordered. No role can grant permissions above its own level.

| Role | Scope | Description |
|---|---|---|
| Universe Operator | Cosmos-wide | Full authority. Owns Universe repo. Approves IAM changes, capability updates, skill promotions, constitution conflicts. |
| Galaxy Owner | One galaxy | Full responsibility for one galaxy. Resolves conflicts when external updates arrive. One owner per galaxy. |
| Galaxy Administrator | One galaxy | Trusted operator. Can develop and operate the galaxy. Assigned by Galaxy Owner. |

**IAM tracking:** A markdown file in the Universe repo. Enforced via GitHub repository permissions on each galaxy's repo.

**Onboarding protocol** — New Galaxy Administrators are onboarded by the Universe agent:
1. Read `universe/galaxy/identity.md` — understand the system
2. Walk through this document
3. Review their galaxy's `galaxy/identity.md` and `galaxy/access.md`
4. Review their role in the Universe IAM markdown
5. Confirm GitHub repo permissions are set correctly
6. First session: scoped task only, no galaxy-level changes until orientation complete

Onboarding is a universal skill. Lives in the Universe. Parameterised by galaxy and role.

---

### 5. Agents

Agents are the only actors in the Cosmos. An agent lives inside one galaxy. It reads, writes, executes, and proposes — but it never grants its own access, never modifies IAM, and never promotes skills to an external source without operator approval.

Planets are stateful storage. Agents are what make planets change.

**Agents do not know the shape of the Cosmos.** A galaxy agent knows its own galaxy. It knows of external planets only if its `galaxy/access.md` declares them — and even then, it does not know which galaxy those planets belong to. All external sources are referred to as external sources only.

**Token-efficient cold start:**

Agents do not load the full galaxy at session start. Cold start is minimal by design.

Mandatory minimum — always loaded:
1. `CONTEXT-MAP.md` — routing table for the session

First action — always:
> *"What is the scope of this session?"*

The answer maps to `CONTEXT-MAP.md`, which routes to the relevant planet directories, `CONTEXT.md` files, skills, and state artifacts for that scope. Nothing else loads unless the task requires it.

Galaxy-level work is detected via trigger strings in `skill-index.yaml` — phrases that indicate the agent needs to load `galaxy/` control plane context (e.g. "update skills", "change conventions", "add planet").

---

### 6. Capabilities

Galaxies are tagged with capability flags. Flags describe what a galaxy does. A galaxy can hold multiple flags and add new ones as it grows — no structural migration required.

**Current capability flags:**

| Flag | Meaning |
|---|---|
| `knowledge-store` | Persistent knowledge planets. Human and agent readable. |
| `software-development` | Code, decisions, branching, deliverables. Active workshop mode. |
| `operational` | Always-on agent. Event-driven. Production or near-production state. |
| `external-users` | Serves people outside the Cosmos. |
| `inter-galactic-source` | Other galaxies may access content from this galaxy's planets. |

All capability declarations live in `universe/capabilities.yaml`. The Universe is the sole authority on what capabilities exist and which galaxy holds them. Galaxies do not declare their own capabilities.

---

### 7. Skills Architecture

Two layers. One router per galaxy.

**Universal skills** — Live in the Universe. Pushed into each galaxy's `galaxy/agent-spec/` via galaxy-sync. Cover infrastructure concerns: ADR creation, inbox processing, HQ updates, git conventions, galaxy operations, skill promotion, onboarding.

**Local skills** — Live in `galaxy/skills/`. Domain-specific. Not shared unless promoted. Examples: financial analysis skills in the financial galaxy, coding workflow skills in tio.

**skill-index.yaml** — Lives in `galaxy/agent-spec/`. Galaxy-owned but Universe-injected. The galaxy agent reads this to understand full capability. Contains trigger strings for task detection. Declares source of each skill (external or local).

```yaml
# skill-index.yaml example
- name: adr
  source: external
  trigger: ["create decision", "log ADR", "we decided"]
  description: Creates a structured Architecture Decision Record

- name: earnings-analysis
  source: local
  trigger: ["analyze earnings", "run financials"]
  description: Runs 5-step financial analysis on a company
```

**galaxy-sync** (Universe-local skill) — Pushes universal skills and runtime updates into member galaxies. If a galaxy removes a Universe-injected skill from its index, the next sync restores it.

**Conflict resolution:** When a sync produces a conflict between an external update and existing galaxy policy, the Galaxy Owner decides which takes precedence. External source does not automatically win. Example: a galaxy operating inside a company environment may have company-mandated git conventions that must not be overridden by universe-level git conventions.

**Skill promotion protocol** — When a local skill proves universally useful:
1. A promotion skill summarizes the skill's purpose and behavior
2. If SSH available: raises a GitHub PR or issue on the Universe repo — PR adds the skill to `universe/inbox/` and a line to `universe/galaxy/hq.md`
3. If no SSH: writes the summary to `outputs/` for manual forwarding by the operator
4. Universe Operator reviews, decides, merges

---

### 8. Bootstrap

Every new galaxy is created from a bootstrap template that lives in the Universe. Bootstrap runs inside the Universe — the Universe agent generates output files into `universe/outputs/`. The operator then copies those files out and executes them in the target environment via a coding agent (claude code, opencode, etc.).

Future: Thunder initiates bootstrap via protocol.

**Bootstrap produces:**
- `CONTEXT-MAP.md` at root (empty routing table, structured)
- `galaxy/` control plane with `identity.md`, `access.md` (empty until external source grants), `agent-spec/` with skill-index pre-populated, `hq.md`, `index.md`, `CONTEXT.md`
- Universal skills injected into `galaxy/agent-spec/` based on declared capabilities
- `inbox/` and `outputs/` directories
- Initial planet scaffold based on declared purpose, each with an empty `CONTEXT.md`

**Bootstrap inputs (5 questions):**
1. Galaxy name and purpose
2. Owner GitHub handle
3. Initial capability flags (from the capability flags list)
4. First planet name and type
5. Whether this galaxy needs access to external planets (access profile seed — external source approves)

The agent can grow the galaxy it lives in from day one. The skeleton is self-evolving.

---

### 9. Migration (v0.1 → v0.2.1)

Each v0.1 galaxy gets a declarative `migration-spec.md`. Not a transformation script. A target state declaration that an agent executes against. The migration spec is cosmos-unaware — it makes no reference to the Universe, other galaxies, or the Cosmos. It describes only the galaxy's own target state.

**What the migration spec covers:**

- **Planet discovery** — how the agent should identify what planets already exist: what directories contain meaningful content, what their purpose is, and how they map to the target planet structure
- **Skill discovery** — how the agent should identify what skills already exist, where they live, and how they map to `galaxy/agent-spec/`
- **Runtime discovery** — how the agent should identify existing agent instructions (conventions, ethics, priorities) and migrate them into `galaxy/agent-spec/`. The agent is informed that runtime may also arrive from an external source in the future — the spec does not name that source
- **Target structure** — the v0.2.1 galaxy skeleton the agent should produce
- **External access** — if this galaxy has been granted access to external planets, the spec notes this without identifying the source. If no access has been granted, this section is absent
- **Known conflicts or exceptions** — galaxy-specific migration notes

Migration is idempotent. Re-runnable. If it fails halfway, run it again against the same spec.

---

### 10. Thunder (Future)

Thunder is the personal universe-level wingman agent. Not implemented in v0.2.1. Placeholder decisions made:

- Thunder operates across personal galaxies — not platform galaxies like t.io
- Platform galaxies get their own purpose-built agents
- Thunder's runtime will live in the Universe galaxy
- Bootstrap via Thunder will be a universe-level protocol when Thunder is mature
- **Speech-to-text** — implementation candidate: OpenAI Whisper
- **Voice activation** — wake phrase: *"wake up, daddy's home"*
- **Music on activation** — The Clash "Should I Stay or Should I Go", AC/DC "Back in Black"

---

## Level 2 — Open Threads

*Acknowledged. Not yet decided. Each is a candidate for a future grilling session.*

---

### OT-1: Token Economics
How token efficiency becomes a first-class galaxy convention. Open questions:
- How does caveman mode get standardised as a universal skill or convention?
- What are the offset/limit read conventions for large files?
- Does the context-map carry line-range hints for targeted reads?
- At what point does a galaxy switch coding agents for token cost reasons (pi.dev)?
- How do we instrument and measure token usage per session (codeburn)?

---

### OT-2: Agent Runtime Spec
How agents think, not just what they do. Open questions:
- What is the ethics framework for galaxy agents?
- What is the ubiquitous language (DDD) for the cosmos? Which terms are canonical?
- What is the plan mode format — how does an agent present a plan before executing?
- Is there a decision council pattern, and what does it look like as a skill?
- What does the mindfulness framework look like — how does an agent check "are we doing the right thing"?
- What is the priority ordering when agent goals conflict?

---

### OT-3: Tooling Conventions
How standard tools are used, not which tools exist. Open questions:
- Git branching strategy per capability flag combination
- Commit message convention (docs, feat, fix, chore, etc.) — universal or galaxy-local?
- GitHub CLI usage patterns — when to raise issues vs PRs vs neither
- Allow/deny action list for agents (e.g. no `git push` without operator confirmation)
- Changelog conventions — what standard files should every galaxy have?

---

### OT-4: Coding Agent Portability
How bootstrap and skills work across different coding agents. Open questions:
- What does bootstrap output look like when targeting opencode vs claude code vs codex vs pi.dev?
- Are there agent-specific profile files projected from `galaxy/agent-spec/`?
- How do skills declare compatibility with specific coding agents?
- What is the migration path if the primary coding agent changes?

---

### OT-5: Galaxy Watcher
How to visualise the Cosmos. Open questions:
- Is a web UI warranted or overkill for a solo developer?
- Can mermaid diagrams in key directories serve as lightweight visualisation?
- What does a galaxy health metric look like — how do we know if a planet has no purpose or roadmap?
- What does the universe-level view look like (all galaxies, their capabilities, their health)?

---

### OT-6: Universe Inbox Routing
How the universe inbox processes incoming proposals. Open questions:
- What is the routing logic when a skill promotion proposal arrives?
- What is the routing logic when a protocol proposal arrives?
- Is there a triage skill, or does the Universe Operator triage manually?
- How do we track proposal status (pending, under review, accepted, rejected)?

---

### OT-7: Git Repo Trackers
A galaxy for tracking the tools and skills ecosystem. Open questions:
- What is the purpose galaxy for tracking upstream tool releases (opencode, skills.sh, etc.)?
- Is this a standalone galaxy or a planet in personal-ops?
- What does the live feed look like — polling, webhook, manual?
- Which repos are worth tracking?

---

### OT-8: Promotion Recognition
How operators and agents recognize when structural promotion is warranted. Open questions:
- What signals suggest a moon has grown into a planet? (size, access frequency, operator judgment?)
- What signals suggest a planet has outgrown its galaxy? (scope creep, capability flag mismatch?)
- Is there a lightweight health check skill that surfaces promotion candidates without automating the decision?
- What does a galaxy spin-out protocol look like — and which parts require Universe Operator vs Galaxy Owner?

---

### OT-9: index.md Relevance
Whether per-planet `CONTEXT.md` files make a galaxy-level `index.md` redundant. Open questions:
- Does the domain-model CONTEXT.md pattern fully replace the file-routing purpose of index.md?
- What is the token yield difference between loading a galaxy-level index vs navigating per-planet CONTEXT files?
- Is the maintenance burden of keeping index.md current worth the navigation benefit?
- Recommendation: evaluate after running the CONTEXT pattern across one real galaxy migration before deciding.

---

### OT-10: Inter-Galaxy Access Protocol
How galaxies access content from external planets in practice. Open questions:
- What does the full pull flow look like technically — how does a galaxy agent fetch from an external planet?
- Does `capabilities.yaml` need to encode which galaxies can talk to which, and which planet names are accessible?
- Should there be a recommended planet naming convention to make inter-galaxy access predictable?
- How does the serving galaxy expose its planets without knowing who is asking?
- Should the Universe maintain a planet registry separate from capabilities.yaml?

---

### OT-11: Migration Instructions for Spec Changes
How future spec updates propagate to existing galaxies. Open questions:
- When the cosmos spec changes, how does the Universe communicate what needs to change in each galaxy?
- Should spec updates ship with a companion migration instruction file?
- How does an agent in an isolated galaxy receive and apply a spec migration if it has no external source contact?
- At what spec version threshold does a migration instruction become mandatory vs optional?

---

## Revision Log

| Version | Date | Summary |
|---|---|---|
| v0.1 | pre-2026-04-25 | Original galaxy system. Single repo. Flat structure. No inter-galaxy model. |
| v0.2 | 2026-04-25 | Cosmos architecture. Universe layer. Capability flags. IAM model. Skills architecture. Token-efficient cold start. Inter-galaxy pull model. Moon/planet promotion model. |
| v0.2.1 | 2026-04-25 | Section reorder (Cosmos→Universe→Galaxy→Operators→Agents). constitution/ dissolved into galaxy/ control plane. CONTEXT pattern adopted. Capability flags updated (software-development, operational). Galaxy cosmic-unawareness formalized. Migration spec rewritten as cosmos-unaware. Bootstrap clarified as Universe-generated output. Inter-galaxy access moved to OT-10. Thunder placeholder decisions added. |
