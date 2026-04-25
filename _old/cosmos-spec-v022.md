# Cosmos Spec v0.2.2
*Genesis document for the second iteration of the galaxy system.*
*Derived from grilling sessions — 2026-04-25*

---

## Index

| Section | Title |
|---|---|
| **Level 1** | **Cosmos Architecture** |
| 1 | Core Concepts |
| 2 | Ontology |
| 3 | Capabilities |
| 4 | Galaxy Structure |
| 5 | Identity Access Management (IAM) Model |
| 6 | Skills Architecture |
| 7 | Token-Efficient Cold Start |
| 8 | Bootstrap |
| 9 | Migration |
| 10 | Onboarding Protocol |
| 11 | Thunder |
| **Level 2** | **Open Threads** |
| OT-1 | Token Economics |
| OT-2 | Agent Runtime Spec |
| OT-3 | Tooling Conventions |
| OT-4 | Coding Agent Portability |
| OT-5 | Galaxy Watcher |
| OT-6 | Universe Inbox Routing |
| OT-7 | Git Repo Trackers |
| OT-8 | Promotion Recognition |
| OT-9 | index.md Relevance |
| OT-10 | Inter-Galaxy Access Protocol |
| OT-11 | Migration Instructions for Spec Changes |

---

## What This Document Is

This is the architecture specification for the Cosmos — a structured system of Artificial Intelligence (AI)-augmented operating environments. It operates at two levels:

**Level 1 — Cosmos Architecture** (resolved): The stable, high-altitude decisions about structure, governance, and relationships between galaxies. These are unlikely to change. They are the constitution of the system.

**Level 2 — Open Threads** (to be grilled): Topics acknowledged but not yet decided. Listed explicitly so future sessions have a clear agenda.

---

## Level 1 — Cosmos Architecture

### 1. Core Concepts

*Important read for all*

**Design principle:** Least privilege applies to information, not just access. Anything that does not need to know something should not know it.

**Cosmos** — The full system. All universes + all galaxies + all agents + all protocols governing how they relate. No single galaxy knows the shape of the Cosmos. No single universe knows the shape of other universes. Awareness is deliberately minimal — a universe only knows itself and its galaxies, a galaxy only knows itself.

**Universe** — A meta-galaxy. The governance layer above a specific set of galaxies. Same structural patterns as galaxies, but its domain is other galaxies — capabilities, Identity Access Management (IAM), universal skills, and the staging/deployment layer between itself and each galaxy it governs. The Universes are the only entities in the Cosmos with full awareness of all galaxies and their relationships.

**Galaxy** — A scoped operating environment. One `git` (version control tool) repository, one clear purpose, one owner. Not a file dump. Not just a document store. A place where work happens, agents operate, and state is tracked. The git repository is the persistence layer. A galaxy has no fixed purpose — it is tagged with capability flags that describe what it does today and can expand as the galaxy grows.

**Agent** — The only actor in the system. Agents live inside galaxies. They read, write, execute, and propose — but they never grant their own access, never modify Identity Access Management (IAM) profiles, and never self-promote skills to an external source without operator approval. Planets are stateful storage. Agents are what make planets change. A galaxy agent knows its own galaxy only. It knows of external planets solely when its access file declares them — and even then, it does not know which galaxy those planets belong to. All external sources are referred to as external sources only.

**Operator** — A human role in the Cosmos. Three tiers exist: Universe Operator (cosmos-wide authority), Galaxy Owner (one galaxy, full responsibility), Galaxy Administrator (one galaxy, trusted developer and operator). Operators are the only entities that can grant access, approve Identity Access Management (IAM) changes, and resolve conflicts between external updates and galaxy policy.

---

### 2. Ontology

*Important read for all*

#### 2.1 Ontology - Industry standard, established conventions applicable outside of this spec

| Object | Role |
|---|---|
| Agent | AI Model, autonomous or prompting. Lives in galaxies and acts within galaxies. |
| Skill | Defined action an agent can do. Universal skills live in Universe. Local skills live in galaxy. |

#### 2.2 Ontology - Cosmos specific

| Object | Role |
|---|---|
| Universe | Meta-galaxy. Governs all others. Authority on Identity Access Management (IAM), capabilities, universal skills. |
| Galaxy | Scoped operating environment. Git repository as persistence layer. One owner. No fixed purpose. |
| Control Plane | The `galaxy/` directory. Identity, access grants, agent runtime, state artifacts. Meta-planet for galaxy-level operations. |
| Planet | Stateful storage within a galaxy. Wiki, code, or operational state. |
| Moon | A sub-topic within a planet. Can be promoted to planet status within the same galaxy. |
| Capability | A declared function a galaxy possesses. Defined and owned by the Universe. |
| Protocol | Codified workflow. How specific operations are executed across the Cosmos. |
| Law | Inter-galaxy or universe-level protocol. Governs cross-boundary operations. |

---

### 3. Capabilities

Galaxies are tagged with capability flags. Flags describe what a galaxy does today. A galaxy can hold multiple flags and add new ones as it grows — no structural migration required. Galaxies do not declare their own capabilities. The Universe is the sole authority.

**Current capability flags:**

*Important read for administrators* ; *Will be extended*

| Flag | Meaning |
|---|---|
| `wiki` | Persistent knowledge bank planets. Human and agent readable. |
| `software-development` | Code, decisions, branching, deliverables. Active workshop mode. |
| `operational` | Production or near-production state. |
| `external-users` | Serves people outside the Cosmos. |
| `inter-galactic-source` | Other galaxies may access content from this galaxy's planets. |
| `autonomous-agents` | Always-on agents. Event-driven. Probably complex environment setup. |

**Where capabilities live:** A single `capabilities.yaml` file in the Universe repository (repo). One file, all galaxies, all tags. The Universe is the only authority on what capabilities exist and which galaxy has them.

```yaml
# universe/capabilities.yaml
galaxies:
  personal-wiki:
    owner: <operator>
    capabilities: [wiki, inter-galactic-source]
  day-job-project1:
    owner: <operator>
    capabilities: [wiki, software-development, operational, external-users]
  tio:
    owner: <operator>
    capabilities: [software-development, knowledge-store, external-users]
  financial:
    owner: <operator>
    capabilities: [knowledge-store, operational]
  personal-ops:
    owner: <operator>
    capabilities: [knowledge-store, software-development]
```

Capability lifecycle — add, modify, deprecate — is managed by a Universe skill and requires code review before deployment.

---

### 4. Galaxy Structure, directory of files - agent friendly

Every galaxy — including the Universe themselves — follows this skeleton:

*Important read for administrators*

```
<galaxy>/
├── CONTEXT-MAP.md           ← root-level agent routing table, loaded at cold start
├── galaxy/                  ← control plane: galaxy-level ops and external push landing zone
│   ├── info.md              ← name, purpose, owner
│   ├── access.md            ← which external planets this galaxy may access (read-only by agent)
│   ├── agent-spec/          ← agent runtime: pushed from external source, projected to providers
│   │   ├── conventions.md
│   │   ├── ethics.md
│   │   ├── skill-index.yaml ← capability router: names, triggers, sources
│   │   └── skills/          ← local galaxy skills
│   ├── hq.md                ← Now / Next / Backlog / Explorations
│   ├── index.md             ← galaxy inventory (see OT-9: may be superseded by CONTEXT.md pattern)
│   └── CONTEXT.md           ← scope routing for galaxy/ control plane
├── inbox/                   ← raw content for processing
├── outputs/                 ← structured outputs from agent work
└── <planet-dirs>/           ← domain-named directories, one per planet
    └── CONTEXT.md           ← planet-level scope routing, file index, agent guidance
```

**The `galaxy/` directory is the control plane.** It is simultaneously a meta-planet (stateful, with its own `CONTEXT.md`) and the landing zone for updates pushed from external sources. Agent runtime, skills, identity, and access grants all live here. External sources push into `galaxy/agent-spec/` — a Universe skill then projects this into provider-specific formats (`.opencode/`, `.claude/`, etc.) for the active coding agent.

**The Context (CONTEXT) pattern:**

- `CONTEXT-MAP.md` at root — the agent's first routing file after the cold start scope question. Maps session scopes to directories and files to load.
- `CONTEXT.md` inside each planet directory — describes what the planet contains, what files live where, and how an agent should navigate it. Reduces the need for directory exploration and file listing at the cost of maintenance.

**The Universe repository additionally contains:**

```
<universe>/
├── CONTEXT-MAP.md           ← agent routing table
├── universe/                ← universe control plane (same skeleton as any galaxy)
│   ├── info.md
│   ├── agent-spec/
│   │   ├── conventions.md
│   │   ├── ethics.md
│   │   └── skill-index.yaml
│   │   └── skills/          ← local skills
│   ├── hq.md
│   ├── index.md
│   └── CONTEXT.md
├── capabilities.yaml        ← single source of truth: all galaxies, all capability flags
├── member-galaxies/         ← one directory per galaxy: IAM profile, access grants, conflict staging
│   ├── personal-wiki/
│   ├── tio/
│   └── <galaxy>/
├── universal-agent-spec/    ← universal library for skills, instructions, runtime, plugins
├── inbox/                   ← skill promotion proposals, protocol proposals
└── outputs/                 ← bootstrap outputs, migration specs, generated files
```

---

### 5. Identity Access Management (IAM) Model

*Important read for all*

Four roles. Strictly ordered. No role can grant permissions above its own level.

| Role | Type | Scope | Description |
|---|---|---|---|
| Universe Operator | Human | Cosmos-wide | Full authority. Owns Universe repository. Approves Identity Access Management (IAM) changes, capability updates, skill promotions, constitution conflicts. |
| Galaxy Owner | Human | One galaxy | Full responsibility for one galaxy. One owner per galaxy. Resolves conflicts when external updates arrive. |
| Galaxy Administrator | Human | One galaxy | Trusted operator. Can develop and operate the galaxy. Assigned by Galaxy Owner. |
| Agent | Artificial Intelligence (AI) | One galaxy | Executes within granted permissions. Cannot self-grant access. |

**Enforcement:** Tracked in a markdown file in the Universe repository. Enforced via GitHub repository permissions on each galaxy's repository.

**Access profile:** Each galaxy has an `access.md` in its `galaxy/` control plane. This file informs the agent which external planets it has been granted access to — meaning it can read content from those planets. It does not move or own those planets. The access profile is authored and maintained by the Universe — not the galaxy agent. Galaxy agents cannot modify their own inter-galactic access grants.

**Granularity:** Planet-level. A galaxy is granted access to specific sets of planets, not to whole galaxies or individual files within planets.

**Model:** Pull only. No galaxy pushes data to another. Requesting galaxies pull what they are granted. No peer-to-peer. All inter-galaxy access is mediated by the access profile.

---

### 6. Skills Architecture

Two layers. One router per galaxy.

#### 6.1 Skill name convention

*Important read for owners*

- ```<location>-skill:/<source or destination>-<action>-<subject>```
  - location = universe, galaxy
  - source or destination = where it ends up
  - action = what we do
  - subject = what we do it on

Note: Skills will be invoked in your agent environment (Claude Code, Gemini Antigravity, etc) with slash `/external-sync-skills`, as normal. The prefix `<location>-skill:` is for agents to parse this spec, generate skills, and understand who the recipient is.

### 6.2 Skill types

*Important read for administrators*

**Universal skills** — Live in the Universe repo. Pushed down to galaxies. Cover infrastructure concerns: ADR creation, inbox processing, HQ updates, git conventions, galaxy operations, skill promotion. Stages skills for galaxies with `universe-skill:/galaxies-stage-skills`

**Local skills** — Live in the galaxy. Domain-specific. Not shared unless promoted. Examples: financial analysis skills in the financial galaxy, coding workflow skills in tio. Fetches universal skills with `galaxy-skill:/external-sync-skills`, includes `skill-index.yaml`.

**skill-index.yaml** — One per galaxy. Galaxy-owned but Universe-injected. The galaxy agent reads this to understand full capability. Contains trigger strings for task detection. Declares source of each skill (universe or local).

### 6.3 Skill details, syntax, conflicts, and promotion

```yaml
# skill-index.yaml example
- name: adr
  source: external
  trigger: ["create decision", "log ADR", "we decided"]
  description: Creates a structured Architecture Decision Record (ADR)

- name: earnings-analysis
  source: local
  trigger: ["analyze earnings", "run financials"]
  description: Runs 5-step financial analysis on a company
```

**galaxy-sync** (Universe-local skill) — Pushes universal skills and runtime updates into member galaxies. If a galaxy removes a Universe-injected skill from its index, the next sync restores it.

**Conflict resolution:** When a sync produces a conflict between an external update and existing galaxy policy, the Galaxy Owner decides which takes precedence. The external source does not automatically win. Example: a galaxy operating inside a company environment may have company-mandated git conventions that must not be overridden by universe-level git conventions.

**Skill promotion protocol** — When a local skill proves universally useful:
1. A promotion skill summarizes the skill's purpose and behavior
2. If Secure Shell (SSH) is available: raises a GitHub Pull Request (PR) or issue on the Universe repository — the Pull Request (PR) adds the skill to `universe/inbox/` and a line to `universe/galaxy/hq.md`
3. If no Secure Shell (SSH) is available: writes the summary to `outputs/` for manual forwarding by the operator
4. Universe Operator reviews, decides, merges

---

### 7. Token-Efficient Cold Start

Agents do not load the full galaxy at session start. Cold start is minimal by design.

**Mandatory minimum — always loaded:**
1. `CONTEXT-MAP.md` — routing table for the session

**First action — always:**
> *"What is the scope of this session?"*

The answer maps to `CONTEXT-MAP.md`, which routes to the relevant planet directories, `CONTEXT.md` files, skills, and state artifacts for that scope. Nothing else loads unless the task requires it.

Galaxy-level work is detected via trigger strings in `skill-index.yaml` — phrases that indicate the agent needs to load `galaxy/` control plane context (e.g. "update skills", "change conventions", "add planet").

---

### 8. Bootstrap

*Important read for everyone*

Every new galaxy is created from a bootstrap template that lives in the Universe. Bootstrap runs inside the Universe — the Universe agent generates output files into `universe/outputs/`. The operator then copies those files out and executes them in the target environment via a coding agent (claude code, opencode, etc.).

Future: Thunder initiates bootstrap via protocol.

**Bootstrap produces:**
- `CONTEXT-MAP.md` at root (empty routing table, structured)
- `galaxy/` control plane with `identity.md`, `access.md` (empty until external source grants access), `agent-spec/` with skill-index pre-populated, `hq.md`, `index.md`, `CONTEXT.md`
- Universal skills injected into `galaxy/agent-spec/` based on declared capabilities
- `inbox/` and `outputs/` directories
- Initial planet scaffold based on declared purpose, each with an empty `CONTEXT.md`

**Bootstrap inputs (5 questions):**
1. Galaxy name and purpose
2. Owner GitHub handle
3. Initial capability flags (from the capability flags list in section 3)
4. First planet name and type
5. Whether this galaxy needs access to external planets (access profile seed — external source approves)

The agent can grow the galaxy it lives in from day one. The skeleton is self-evolving.

---

### 9. Migration (v0.1 → v0.2.2)

*Important read for administrators*

Each version 0.1 (v0.1) galaxy gets a declarative `migration-spec.md`. Not a transformation script. A target state declaration that an agent executes against. The migration spec is Cosmos-unaware — it makes no reference to the Universe, other galaxies, or the Cosmos. It describes only the galaxy's own target state from the galaxy's own perspective.

**What the migration spec covers:**

- **Planet discovery** — how the agent should identify what planets already exist: what directories contain meaningful content, what their purpose is, and how they map to the target planet structure
- **Skill discovery** — how the agent should identify what skills already exist, where they live, and how they map to `galaxy/agent-spec/skills/`
- **Runtime discovery** — how the agent should identify existing agent instructions (conventions, ethics, priorities) and migrate them into `galaxy/agent-spec/`. The agent is informed that runtime may also arrive from an external source in future — the migration spec doesn't mention what source, but the cosmos-spec knows it from the universe.
- **Target structure** — the v0.2.2 galaxy skeleton the agent should produce
- **External access** — if this galaxy has been granted access to external planets, the spec notes this without identifying the source. If no access has been granted, this section is absent
- **Known conflicts or exceptions** — galaxy-specific migration notes

Migration is idempotent. Re-runnable. If it fails halfway, run it again against the same spec.

**Promotion model:**

Moons are not permanent sub-citizens. A moon that grows in scope or complexity can be promoted to a planet within the same galaxy. This is an intra-galaxy operation — a skill, not a manual file shuffle. Moon to Planet (M→P) promotion never requires Universe involvement.

When a planet outgrows its galaxy entirely, a galaxy spin-out may be warranted. This is a separate, heavier operation: new bootstrap, Universe registration, Identity Access Management (IAM) profile creation. It is a protocol, not a skill. Neither promotion type is triggered on a schedule — both are judgment calls made by the operator and agent when context demands it. We cannot foresee the contexts that will make these decisions necessary.

---

### 10. Onboarding Protocol

*Important read for everyone*

New Galaxy Administrators receive a structured onboarding facilitated by the Universe agent:

1. Read the cosmos specification document
2. Read `./galaxy/identity.md` — understand your system
3. Review their galaxy's `galaxy/identity.md` and `galaxy/access.md`
4. Review their role in the Universe Identity Access Management (IAM) markdown
5. Confirm GitHub repository permissions are set correctly
6. First session: scoped task only, no galaxy-level changes until orientation is complete

Onboarding is a universal skill. Lives in the Universe. Parameterised by galaxy and role.

---

### 11. Thunder (Future)

Thunder is the personal wingman agent. Not implemented in v0.2.2. Placeholder decisions made:

- Thunder operates across personal galaxies — not platform galaxies like tio
- Platform galaxies get their own purpose-built agents
- Thunder's runtime will live in the Universe galaxy
- Bootstrap via Thunder will be a universe-level protocol when Thunder is mature
- **Speech-to-text** — implementation candidate: https://github.com/cjpais/Handy 
- **Voice activation** — wake phrase: *"wake up, daddy's home" (from movie - Iron Man 2)* 
- **Music on activation** — The Clash "Should I Stay or Should I Go" *(Iron Man 2)*, AC/DC "Back in Black"

---

## Level 2 — Open Threads

*Acknowledged. Not yet decided. Each is a candidate for a future grilling session.*

---

### OT-1: Token Economics
How token efficiency becomes a first-class galaxy convention. Open questions:
- How does caveman mode get standardised as a universal skill or convention?
- What are the offset/limit read conventions for large files?
- Does the Context Map (CONTEXT-MAP) carry line-range hints for targeted reads?
- At what point does a galaxy switch coding agents for token cost reasons (pi.dev)?
- How do we instrument and measure token usage per session (codeburn)?

---

### OT-2: Agent Runtime Spec
How agents think, not just what they do. Open questions:
- What is the ethics framework for galaxy agents?
- What is the ubiquitous language for the Cosmos under Domain Driven Design (DDD)? Which terms are canonical?
- What is the plan mode format — how does an agent present a plan before executing?
- Is there a decision council pattern, and what does it look like as a skill?
- What does the mindfulness framework look like — how does an agent check "are we doing the right thing"?
- What is the priority ordering when agent goals conflict?

---

### OT-3: Tooling Conventions
How standard tools are used, not which tools exist. Open questions:
- Git branching strategy per capability flag combination
- Commit message convention (docs, feat, fix, chore, etc.) — universal Law or galaxy-local?
- GitHub Command Line Interface (CLI) usage patterns — when to raise issues vs Pull Requests (PRs) vs neither
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
- Is a web User Interface (UI) warranted or overkill for a solo developer?
- Can Mermaid diagrams in key directories serve as lightweight visualisation?
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

### OT-7: Git Repository (Repo) Trackers
A galaxy for tracking the tools and skills ecosystem. Open questions:
- What is the purpose galaxy for tracking upstream tool releases (opencode, skills.sh, etc.)?
- Is this a standalone galaxy or a planet in personal-ops?
- What does the live feed look like — polling, webhook, manual?
- Which repositories are worth tracking?

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
- Does the domain-model `CONTEXT.md` pattern fully replace the file-routing purpose of `index.md`?
- What is the token yield difference between loading a galaxy-level index vs navigating per-planet Context (CONTEXT) files?
- Is the maintenance burden of keeping `index.md` current worth the navigation benefit?
- Recommendation: evaluate after running the Context (CONTEXT) pattern across one real galaxy migration before deciding.

---

### OT-10: Inter-Galaxy Access Protocol
How galaxies access content from external planets in practice. Open questions:
- What does the full pull flow look like technically — how does a galaxy agent fetch from an external planet?
- Does `capabilities.yaml` need to encode which galaxies can talk to which, and which planet names are accessible?
- Should there be a recommended planet naming convention to make inter-galaxy access predictable?
- How does the serving galaxy expose its planets without knowing who is asking?
- Should the Universe maintain a planet registry separate from `capabilities.yaml`?

---

### OT-11: Migration Instructions for Spec Changes
How future spec updates propagate to existing galaxies. Open questions:
- When the Cosmos spec changes, how does the Universe communicate what needs to change in each galaxy?
- Should spec updates ship with a companion migration instruction file?
- How does an agent in an isolated galaxy receive and apply a spec migration if it has no external source contact?
- At what spec version threshold does a migration instruction become mandatory vs optional?

---

## Revision Log

| Version | Date | Summary |
|---|---|---|
| v0.1 | pre-2026-04-25 | Original galaxy system. Single repository. Flat structure. No inter-galaxy model. |
| v0.2 | 2026-04-25 | Cosmos architecture. Universe layer. Capability flags. Identity Access Management (IAM) model. Skills architecture. Token-efficient cold start. Inter-galaxy pull model. Moon/planet promotion model. |
| v0.2.1 | 2026-04-25 | Section reorder. constitution/ dissolved into galaxy/ control plane. Context (CONTEXT) pattern adopted. Capability flags updated. Galaxy cosmic-unawareness formalized. Migration spec rewritten as Cosmos-unaware. Bootstrap clarified as Universe-generated output. Inter-galaxy access moved to Open Thread (OT-10). Thunder placeholder decisions added. |
| v0.2.2 | 2026-04-25 | Section order corrected (Core Concepts → Ontology → Capabilities → Galaxy Structure → Identity Access Management (IAM) → Skills → Cold Start → Bootstrap → Migration → Onboarding → Thunder). Spec index added. All abbreviations spelled out on first use. Onboarding restored as standalone section. |
