# Old HQ
<!-- INDEX — use offset/limit for targeted reads
  Now         L11  active this session (1-3 items)
  Next        L28  queued, clear scope, no blockers
  Backlog     L53  defined work, no commitment — subsections: Analysis, System & Vault, Personal
  Someday     L118 no timeline
  Explorations L128 ideas, questions, loose threads
  Total lines: ~138
-->

### Now
*1–3 items max — actively in progress this session*

- [ ] Investigate token efficiency — reduce token usage in current sessions and design token-efficient patterns into ai-plane skills (context loading, offset/limit reads, skill scoping) (2026-04-21)
	- [ ] https://github.com/juliusbrussee/caveman , less words and usage of symbols
- [ ] Agentic-stack, .agent/ directory 
	- [ ] https://github.com/codejunkie99/agentic-stack
- [ ] Mermaid diagrams in galaxy-watcher.md, where we can create visualization for galaxy insights. Each diagram needs a text explanation description, which the mermaid syntax is based on. 
- [ ] Implement ai-plane architecture from Outputs/ai-plane-development.md — create routing layer, profiles system, restructure skills into directory convention (2026-04-23)
    - [ ] Create ai-plane/routing/ directory and skill-index.yaml
    - [ ] Create ai-plane/profiles/ directory and first profile
    - [ ] Migrate skills/adr.md → skills/adr/SKILL.md (directory-based structure)
    - [ ] Update ai-plane/STATUS.md to reflect current state
    - [ ] Archive Outputs/ai-plane-development.md after implementation complete

---

### Next
*Queued and ready to start — clear scope, no blockers*

- [ ] 
- [ ] Skills are reusable capabilities for AI agents. Install them with a single command to enhance your agents with access to procedural knowledge.
	- [ ] https://skills.sh/
- [ ] free open source UI library
	- [ ] https://ui.shadcn.com/
- [ ] AI Plane Implementation — build skill router and migrate reusable Claude skills from vault (2026-04-21)
    - [ ] Decide skill granularity (single vs. multiple routable skills)
    - [ ] Process inbox templates (ai skills directory.md, Level up claude skills.md)
    - [ ] Create ai-plane/skills/ directory structure
    - [ ] Build skill index with candidates (Analysis, Business Library, Cognitive)
    - [ ] Migrate first skill (5 Step System or Business Evaluation)
- [/] Update CLAUDE.md to incorporate context/memory files — galaxy-index.md, galaxy-journal.md, and ai-plane/STATUS.md as formal context sources Claude reads at session start (2026-04-21)
    - [x] ADR skill added to CLAUDE.md read instructions (2026-04-22)
    - [ ] Define how Claude should use galaxy-index.md for vault orientation
    - [ ] Define how galaxy-journal.md provides session history and continuity
    - [ ] Reference ai-plane/STATUS.md as the live AI plane planning document
    - [ ] Consider referencing the 4 ai-plane foundation framework files as persistent context
- [ ] Review Level Up Claude skills — 8 approaches for mastering Claude (voice profiles, cowork folder, global instructions, connectors, plugins) (2026-04-20)
    - [i] inbox/Level up claude skills.md

---

### Backlog
*Defined work — no immediate commitment*

**Analysis**
- [ ] Aurora Innovation (AUR) Market Analysis — understand trucking market, Aurora's positioning, and competitive forces (2026-04-21)
    - [ ] Understand USA trucking market structure and dynamics
    - [ ] Determine if Aurora leans hardware vs software
    - [ ] Determine if Aurora leans trucking infrastructure vs software
    - [ ] Map Aurora's position in value chain (truck operability only, or broader scope)
    - [ ] Analyze competitive forces and market structure
    - [ ] Use 5 Step Framework for company analysis
    - [ ] Use Socratic Tutor to deepen understanding of key concepts
- [ ] Nordic Iron Ore (STO:NIO) Financial Analysis — assess company fundamentals and CEO alignment (2026-04-21)
    - [ ] Conduct financial analysis using 5 Step System
    - [ ] Research CEO shareholding — determine if CEO has skin in the game
    - [ ] Interpret CEO share ownership as signal of company belief
    - [ ] Analyze overall investment thesis
- [ ] Understand order volume analysis — learn how order volume correlates with stock price, create graphs for insight extraction (2026-04-21)
    - [ ] Study order volume patterns and market impact
    - [ ] Build visualization tools (volume bars, volume trends, price correlation)
    - [ ] Identify actionable signals from volume analysis
- [ ] Build mathematical quantitative financial analysis tools — have AI suggest tools, create them, then quiz on understanding (math behind MACD, RSI, Bollinger Bands, etc.) (2026-04-21)
    - [ ] Define scope: which technical analysis algorithms to learn
    - [ ] Create interactive tool(s) for MACD, RSI, Stochastic, Bollinger Bands
    - [ ] Learning quiz: AI tests comprehension of math and interpretation
- [ ] Supply Chain Framework — research cross-industry structures and competitive advantage patterns (2026-04-20)
    - [ ] Phase 1: Identify competitive advantage vs industry norms
    - [ ] Phase 2: Define AI-compatible input schema and time-layer methodology
    - [ ] Phase 3: Validate against 3-5 industry case studies
- [ ] Build Business Evaluation System — integrate Seven Questions, Monopoly Dynamics, Incentives, Alignment, Distribution into repeatable framework (2026-04-20) — from [[Zero To One - Peter Thiel]]
    - [ ] Define scoring criteria for each dimension
    - [ ] Create AI-compatible prompt template
    - [ ] Test against 3-5 company case studies
- [ ] Analyze Slate Star Codex Zero To One review against own notes — compare insights and identify gaps (2026-04-20) — from [[Zero To One - Peter Thiel]]
    - [i] https://slatestarcodex.com/2019/01/31/book-review-zero-to-one/
- [ ] Read political articles in politics project — analyze Swedish deep state and US political economy (2026-04-21)
    - [ ] Read Projects/politics/Sweden/Samnytt - Swedish Deep State.md
    - [ ] Read Projects/politics/USA/Memdani.md
    - [ ] Extract key insights and cross-reference patterns

**System & Vault**
- [ ] Vault galaxy visualization — graphical tool to show vault structure, file connections, and active work state (2026-04-21)
	- [ ] measure health, all files must have a purpose and roadmap, if the file roadmap not done, unhealthy file
	- [ ] This is very important in order to get an accurate overview of what is happening and the direction
    - [ ] Define what to visualize (files, wiki links, active todos, clusters, ai-plane layer)
    - [ ] Evaluate tooling options (Obsidian graph view, custom web tool, D3.js, other)
    - [ ] Build or configure the tool
- [ ] Plan .claude/ skills directory restructuring — organize skill files into standard convention (2026-04-20)
    - [i] inbox/move to skills directory standard.md
- [ ] Add a AI skill specification for how to deal with git, not only when to commit and branch, but also git release flows. Github flow, googles "release-please"
	- [ ] [[Git flow strategies]]
- [ ] git commit message start with conventions: docs, feat, fix, chore, etc.
- [ ] Build ai-plane Maturity Model for AI Instructions — define levels of instruction clarity (low → high maturity) evaluating completeness, precision, and context-awareness (2026-04-22)
    - [i] From "The Brain: The Story of You" — Free Will section (p. 104)
- [ ] Create ai-plane conventions log — document which tools we use, why we use them, and how we intend to use them; covers decisions below ADR threshold (recurring patterns, operating conventions) (2026-04-22)

- [ ] Implement about-me.md — define routine daily activities and habits to build into day (2026-04-21)

**Personal**
- [ ] Bedroom interior design — plan and execute design updates (2026-04-21)
- [ ] AI Tool for summarizing YouTube videos (2026-04-20)
	- [ ] https://github.com/obra/Youtube2Webpage
    - [i] https://www.getrecall.ai/pricing

---

### Someday
No timeline — revisit when relevant

- [ ] Speech-to-text mic system — candidates: github.com/cjpais/Handy, openai/whisper
- [ ] Feynman multi-agent workflow in ai-plane — [<] blocked: needs skill router + populated skills/ — ai-plane/future-capabilities/feynman-research-agent.md
- [ ] Voice activation — "wake up, daddys home" wake phrase
- [ ] Music playback — local MP3s: The Clash "Should I Stay or Should I Go", AC/DC "Back in Black"

---

### Explorations
*Ideas, questions, loose threads*

- [I] Incentive framework beyond companies — politics, platforms, economies — [[Zero To One - Peter Thiel]]
- [I] Nihilism & Dogmatism — philosophy, epistemology, org psychology — [[Zero To One - Peter Thiel]]
- [I] Map "invisible infrastructure" — aggregated distribution nodes in fragmented markets — [[Zero To One - Peter Thiel]]
- [?] How to have successful meetings?
- [?] AI picks-and-shovels — who benefits supplying AI competition? — [[Zero To One - Peter Thiel]]
- [?] AI reward systems — adjustable or fixed? User control? Regulation needed? — [[The Brain - The Story of You - David Eagleman]]
- [I] Emotional intensity vs political ideology — sensitivity or regulation? — [[The Brain - The Story of You - David Eagleman]]
- [I] Personal "common currency" — what metric to evaluate options? (time, money, leverage, learning) — [[The Brain - The Story of You - David Eagleman]]
