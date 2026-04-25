# Cosmos Spec v0.2
*Genesis document for the second iteration of the galaxy system.*
*Derived from grilling session — 2026-04-25*

---

## What This Document Is

This is the architecture specification for the Cosmos — a structured system of AI-augmented operating environments. It operates at two levels:

**Level 1 — Cosmos Architecture** (resolved): The stable, high-altitude decisions about structure, governance, and relationships between galaxies. These are unlikely to change. They are the constitution of the system.

**Level 2 — Open Threads** (to be grilled): Topics acknowledged but not yet decided. Listed explicitly so future sessions have a clear agenda.

---

## Level 1 — Cosmos Architecture

### 1. Core Concepts

**Galaxy** — A scoped operating environment. One git repository, one clear purpose, one owner. Not a file dump. Not just a knowledge store. A place where work happens, agents operate, and state is tracked. The git repository is the persistence layer.

**Universe** — A meta-galaxy. The governance layer above all galaxies. Same structural patterns as a galaxy, but its planets are other galaxies. It is the authority on IAM, capabilities, and universal skills.

**Agent** — Agents live inside galaxies. They read, write, execute, and propose — but they never grant their own access, never modify IAM profiles, and never self-promote skills to the universe without operator approval. Planets are stateful storage. Agents are what make planets change.

**Cosmos** — The full system. Universe + all galaxies + all agents + all protocols governing how they relate.

---

### 2. Ontology

#### 2.1 Ontology - Industry standard, established conventions.
| Object | Role |
|---|---|
| Agent | AI Model, autonomous or prompting. Lives in galaxies and acts within galaxies. |
| Skill | Industry standard defined action an agent can do. Universal skills live in Universe. Local skills live in galaxy. |

#### 2.2 Ontology - Cosmos specific

| Object | Role |
|---|---|
| Universe | Meta-galaxy. Governs all others. Authority on IAM, capabilities, universal skills. |
| Galaxy | Scoped operating environment. Git repo as persistence. One owner. |
| Constitution | Galaxy-level directory. Identity, context-map, access profile. |
| Planet | Stateful storage within a galaxy. Knowledge, code, or operational state. |
| Capability | A declared function a galaxy possesses. Defined in the Universe. |
| Protocol | Codified workflow. How specific operations are executed across the cosmos. |
| Law | Inter-galaxy or universe-level protocol. Governs cross-boundary operations. |


---

### 3. Galaxy Capabilities

Galaxies are not typed. They are tagged with capabilities. A galaxy can hold multiple capabilities and evolve over time by adding new ones without migration.

**Capability flags (current):**

| Flag | Meaning |
|---|---|
| `knowledge-store` | Persistent knowledge planets. Human and agent readable. |
| `active-development` | Workshop mode. Code, decisions, branching, deliverables. |
| `external-users` | Serves people outside the cosmos. |
| `autonomous-ops` | Always-on agent. Event-driven. Not voice-triggered. |
| `inter-galactic-source` | Other galaxies may pull from this galaxy's planets. |

**Where capabilities live:** A single `capabilities.yaml` file in the Universe repo. One file, all galaxies, all tags. The Universe is the only authority on what capabilities exist and which galaxy has them.

```yaml
# universe/capabilities.yaml
galaxies:
  knowledge-bank:
    owner: <operator>
    capabilities: [knowledge-store, inter-galactic-source]
  tio:
    owner: <operator>
    capabilities: [active-development, knowledge-store]
  financial:
    owner: <operator>
    capabilities: [knowledge-store, autonomous-ops]
  personal-ops:
    owner: <operator>
    capabilities: [knowledge-store, active-development]
```

The Universe parses this file to determine what to push to each galaxy. Capability lifecycle (add, modify, deprecate) is managed by a local Universe skill. Requires code review before propagation.

---

### 4. Galaxy Structure

Every galaxy — including the Universe itself — follows this skeleton:

```
<galaxy>/
├── constitution/
│   ├── identity.md          ← name, purpose, owner, galaxy link in universe
│   ├── context-map.md       ← scope-to-files routing table for agents
│   └── access-profile.md    ← what this galaxy can pull from others (read-only by galaxy agent)
├── skill-index.yaml         ← capability router: names, triggers, sources
├── skills/                  ← local galaxy skills
├── hq.md                    ← Now / Next / Backlog / Explorations
├── index.md                 ← galaxy inventory
├── inbox/                   ← raw content for processing
├── outputs/                 ← structured outputs from agent work
└── planets/                 ← or domain-named root dirs (e.g. books/, research/)
```

The Universe repo additionally contains:

```
universe/
├── capabilities.yaml        ← single source of truth for all galaxy capabilities
├── skills/                  ← universal skills library
├── galaxies/
│   ├── knowledge-bank/      ← IAM profile, access grants, conflict staging area
│   ├── tio/                 ← IAM profile, pending updates, conflict log
│   └── <galaxy>/            ← one directory per galaxy
└── inbox/                   ← skill promotion proposals, protocol proposals
```

---

### 5. IAM Model

Four roles. Strictly ordered. No role can grant permissions above its own level.

| Role | Type | Scope | Description |
|---|---|---|---|
| Universe Operator | Human | Cosmos-wide | Full authority. Owns Universe repo. Approves IAM changes, skill promotions, constitution conflicts. |
| Galaxy Owner | Human | One galaxy | Full responsibility for a galaxy. One per galaxy. Resolves constitution conflicts on pull. |
| Galaxy Administrator | Human | One galaxy | Trusted operator. Can develop and operate the galaxy. Assigned by Galaxy Owner. |
| Agent | AI | One galaxy | Executes within granted permissions. Cannot self-grant access. |

**Enforcement:** Tracked in a markdown file in the Universe repo. Enforced via GitHub repository permissions on the corresponding galaxy repo.

**Access profile:** Each galaxy has an `access-profile.md` in its constitution directory. This defines what planets it can pull from other galaxies. The access profile is authored and maintained by the Universe — not the galaxy agent. Galaxy agents cannot modify their own inter-galactic access.

**Granularity:** Planet-level. A galaxy is granted access to whole planets, not individual moons or files within them.

**Model:** Pull only. No galaxy pushes data to another. Requesting galaxies pull what they are granted. No peer-to-peer. All inter-galaxy access is mediated by the access profile.

---

### 6. Skills Architecture

Two layers. One router per galaxy.

#### Skill name convention

- ```<location>-skill:/<source or destination>-<action>-<subject>```
  - location = universe, galaxy
  - source or destination = where it ends up
  - action = what we do
  - subject = what we do it on

Note: Skills will be invoked in your agent environment (Claude Code, Gemini Antigravity, etc) with slash `/external-sync-skills`, as normal. The prefix `<location>-skill:` is for agents to parse this spec, generate skills, and understand who the recipient is.

###

**Universal skills** — Live in the Universe repo. Pushed down to galaxies. Cover infrastructure concerns: ADR creation, inbox processing, HQ updates, git conventions, galaxy operations, skill promotion. Stages skills for galaxies with `universe-skill:/galaxies-stage-skills`

**Local skills** — Live in the galaxy. Domain-specific. Not shared unless promoted. Examples: financial analysis skills in the financial galaxy, coding workflow skills in tio. Fetches universal skills with `galaxy-skill:/external-sync-skills`, includes `skill-index.yaml`.

**skill-index.yaml** — One per galaxy. Galaxy-owned but Universe-injected. The galaxy agent reads this to understand full capability. Contains trigger strings for task detection. Declares source of each skill (universe or local).

```yaml
# skill-index.yaml example entry
- name: adr
  source: universe
  trigger: ["create decision", "log ADR", "we decided"]
  description: Creates a structured Architecture Decision Record

- name: earnings-analysis
  source: local
  trigger: ["analyze earnings", "run financials"]
  description: Runs 5-step financial analysis on a company
```

**Skill promotion protocol** — When a local skill proves universally useful:
1. A promotion skill summarizes the skill's purpose and behavior
2. If SSH available: raises a GitHub PR or issue on the Universe repo (PR adds file to universe/inbox/ and universe/hq.md)
3. If no SSH: writes summary to galaxy outputs/ for manual forwarding
4. Universe Operator reviews, decides, merges

---

### 7. Token-Efficient Cold Start

Agents do not load the full galaxy at session start. Cold start is minimal by design.

**Mandatory minimum context (always loaded):**
1. `constitution/identity.md` — who this galaxy is and what it does
2. `skill-index.yaml` — what the agent can do and what triggers what

**Session scoping (always asked before loading more):**
> *"What is the scope of this session?"*

The answer maps to `constitution/context-map.md`, which routes to the relevant planet files, skills, and state artifacts for that scope. Nothing else loads unless the task requires it.

**Galaxy-level work detection:** Trigger strings in skill-index.yaml catch phrases that require loading the full constitution directory (e.g. "update capabilities", "add planet", "change access profile").

---

### 8. Inter-Galaxy Communication

**Model:** Pull only. No event push. No peer-to-peer.

**Flow:**
1. Galaxy agent determines it needs knowledge from another galaxy
2. Checks own `access-profile.md` to confirm the planet is accessible
3. Fetches from the source galaxy's planet directory
4. Never writes to another galaxy

**Conflict staging:** The Universe repo's `galaxies/<name>/` directory acts as a staging layer. Before universe-level changes propagate to a galaxy, conflicts are resolved here. The Galaxy Owner (human) resolves constitution conflicts manually. This mirrors the EU legislative model — directives are staged before reaching member states.

**IAM grant changes** require Universe Operator approval. Requested via universe inbox. Never self-initiated by a galaxy agent.

---

### 9. Bootstrap Template

Every new galaxy is created from the Universe bootstrap template. Bootstrap is run by the Galaxy Owner via a coding agent (claude code, opencode, etc.). Future: Thunder initiates bootstrap via protocol.

**Bootstrap produces:**
- `constitution/` directory with identity, context-map (empty), access-profile (empty until Universe grants)
- `skill-index.yaml` pre-populated from capabilities declared at bootstrap time
- Universal skills injected based on capabilities
- `hq.md` and `index.md` state artifacts (empty, structured)
- `inbox/` and `outputs/` directories
- Initial planet scaffold based on declared purpose

**Bootstrap inputs (5 questions):**
1. Galaxy name and purpose
2. Owner GitHub handle
3. Initial capabilities (from capability flags list)
4. First planet name and type
5. Which galaxies it needs to pull from (access profile seed — Universe approves)

The agent can grow the galaxy it lives in from day one. The skeleton is self-evolving.

---

### 10. Migration (v0.1 → v0.2)

Each v0.1 galaxy gets a declarative `migration-spec.md`. Not a transformation script. A target state declaration that an agent executes against.

**Migration spec contains:**
- Galaxy identity (name, purpose, owner)
- Existing planets — what they are and what they contain
- Target capabilities to declare in `universe/capabilities.yaml`
- Target constitution structure
- Skills to migrate or deprecate
- Known conflicts or exceptions

Migration is idempotent. Re-runnable. If it fails halfway, run it again against the same spec.

---

### 11. Onboarding Protocol

New Galaxy Administrators receive a structured onboarding facilitated by the Universe agent:

1. Read `universe/constitution/identity.md` — understand the cosmos
2. Walk through the ontology (this document)
3. Review their galaxy's `constitution/identity.md` and `access-profile.md`
4. Review their role in the Universe IAM markdown
5. Confirm GitHub repo permissions are set correctly
6. First session: scoped task only, no galaxy-level changes until orientation is complete

Onboarding is a universal skill. Lives in the Universe. Parameterised by galaxy and role.

---

### 12. Thunder (Future)

Thunder is the personal universe-level wingman agent. Not implemented in v0.2. Placeholder decisions:

- Thunder operates across personal galaxies (not platform galaxies like t.io)
- Platform galaxies get their own purpose-built agents
- Thunder's runtime will live in the Universe galaxy
- Voice activation and wake phrase are implementation concerns for a future grilling session
- Bootstrap via Thunder will be a universe-level protocol when Thunder is mature

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
- Git branching strategy per galaxy type / capability combination
- Commit message convention (docs, feat, fix, chore, etc.) — universal Law or galaxy convention?
- GitHub CLI usage patterns — when to raise issues vs PRs vs neither
- Allow/deny action list for agents (e.g. no `git push` without operator confirmation)
- Changelog conventions — what standard files should every galaxy have?

---

### OT-4: Coding Agent Portability
How the bootstrap and skills work across different coding agents. Open questions:
- What does the bootstrap look like when run in opencode vs claude code vs codex vs pi.dev?
- Are there agent-specific profile files (like v0.1 `profiles/claude-code.md`)?
- How do skills declare compatibility with specific agents?
- What is the migration path if we switch primary coding agent?

---

### OT-5: Galaxy Watcher
How to visualise the cosmos. Open questions:
- Is a web UI warranted or overkill for a solo developer?
- Can mermaid diagrams in key directories serve as lightweight visualisation?
- What does a galaxy health metric look like — how do we know if a file has no purpose or roadmap?
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

## Revision Log

| Version | Date | Summary |
|---|---|---|
| v0.1 | pre-2026-04-25 | Original galaxy system. Single repo. Flat structure. No inter-galaxy model. |
| v0.2 | 2026-04-25 | Cosmos architecture. Universe layer. Capability flags. IAM model. Skills architecture. Token-efficient cold start. Inter-galaxy pull model. |
