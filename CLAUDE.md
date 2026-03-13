# Agent Instructions

> This file is mirrored across CLAUDE.md, AGENTS.md, and GEMINI.md so the same instructions load in any AI environment.

## Getting Started

### Prerequisites

- Python 3.10+
- Node.js 18+ and npm
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed
- [Modal](https://modal.com/) account (for cloud webhooks)
- Google Cloud project with OAuth credentials (for Gmail/Sheets skills)

### Setup

1. **Clone and install dependencies:**
   ```bash
   pip install -r requirements.txt
   npm install
   ```

2. **Configure environment variables:**
   ```bash
   cp .env.example .env
   ```
   Then edit `.env` and fill in your API keys for each service you plan to use.

3. **Run Claude Code:**
   ```bash
   claude
   ```
   Skills auto-activate based on your requests.

---

## The Skills Architecture

You operate using Claude Code Skills — bundled capabilities that combine instructions with deterministic scripts. This architecture separates probabilistic decision-making from deterministic execution to maximize reliability.

**Layer 1: Skills (Intent + Execution bundled)**
- Live in `.claude/skills/`
- Each Skill = `SKILL.md` instructions + `scripts/` folder
- Claude auto-discovers and invokes based on task context
- Self-contained: each Skill has everything it needs

**Layer 2: Orchestration (Decision making)**
- Claude's job: intelligent routing
- Read SKILL.md, run bundled scripts in the right order
- Handle errors, ask for clarification, update Skills with learnings
- The glue between intent and execution

**Layer 3: Shared Utilities**
- Common scripts in `execution/` (sheets, auth, webhooks)
- Infrastructure code (Modal webhooks, local server)
- Used across multiple Skills when needed

**Why this works:** if you do everything yourself, errors compound. 90% accuracy per step = 59% success over 5 steps. The solution is push complexity into deterministic code. That way Claude just focuses on decision-making.

## Available Skills (22 total)

### Skill Routing Map

Use this map to pick the right skill. When multiple skills could apply, the map resolves ambiguity.

| Skill | What it is | Audience | Voice | Core Artifact |
|---|---|---|---|---|
| `writing-style` | **HOW** you write (universal style guide) | Any recipient | "I" | Tone, sentence construction, density. Data-grounded claims, direct quotes as evidence, root cause analysis, mechanisms over aspirations. |
| `white-paper` | Build a **business case**, align on WHAT to build | Cross-functional teams | "We" | Dollar-quantified problem → scoped MVPs → dependencies table |
| `vision-document` | Set multi-year **direction** | Leadership / org-wide | "We" | Page 0 data dashboard → tenets → pain points → phased MLP with metrics targets → FAQs → appendix |
| `brd-prd` | Specify **WHAT to build** for engineering | Engineering / product | "We" | Overview → current state → MVP experience → prioritization (P0/P0.5/P1/P2) → epics & user stories → launch phasing → FAQs |
| `technical-analysis` | Surface WHERE **data is broken** and WHY | Engineering | "We" | SQL/methodology → field mismatch tables → root causes → tracked tickets |
| `launch-plan` | Ensure **readiness** for a high-stakes event | Ops / engineering / leadership | "We" | System assessment → stress test → war room → phased plan → contacts |
| `30-60-90-day-plan` | Personal **onboarding/ramp plan** | Hiring manager / leadership | "I" | Learn → Execute → Scale. Commitments, assumptions, questions per phase. |
| `strategy-memo` | Frame a **strategic decision** | Leadership | "We" | One-pager: context → recommendation → tradeoffs |
| `executive` | **Advisor persona** for framing PM work | You (coaching) | N/A | Not a template — a subagent you ask for strategic communication advice |
| `project-status` | **Status update** for leadership | Leadership | "We" | Status sentence → data-grounded completed items → active next steps with dates → honest risks |
| `user-researcher` | Synthesize **user research** | Product team | "We" | Pain points, personas, insight synthesis |
| `seller-pitch` | Build a **seller-specific pitch** for a service | Prospective seller | "We/You" | Pain diagnosis → quantified benefits → economics → transition plan → close |
| `presentation` | Convert markdown → **polished slide deck** via Gamma API | Any audience | N/A | Gamma URL + PPTX/PDF export. Pairs with any document skill. |
| `ux-mocks` | **UX mockups** via Stitch AI or manual Tailwind | Product / design | N/A | HTML mock + Figma export. Stitch-assisted or manual. |
| `frontend-design` | **HOW** frontends look (cross-cutting aesthetic layer) | Any frontend work | N/A | Typography, color, motion, spatial composition. |
| `credit-card-recommender` | **Credit card** recommendation engine | Personal use | N/A | Portfolio parser → issuer rules → offer scraper → ranked recs → application guidance |

**How `writing-style` interacts with other skills:** Writing-style is the cross-cutting style layer. It governs voice, tone, sentence construction, and what to avoid across ALL writing. When a document-specific skill is also active, the document skill's structure and voice override writing-style where they conflict. Writing-style still governs sentence construction, content density, and banned words/phrases.

**How `frontend-design` interacts with other skills:** Frontend-design is the cross-cutting aesthetic layer for all frontend work — the visual equivalent of `writing-style`. It governs typography, color, motion, spatial composition, and backgrounds. When `ux-mocks` is also active, `ux-mocks` decides WHAT to build and `frontend-design` decides HOW it looks.

**Document lifecycle flow:** `vision-document` (where are we going?) → `white-paper` (why should we invest?) → `brd-prd` (what exactly do we build?) → `launch-plan` (are we ready to ship?). Each doc type feeds the next. Any document can be converted to slides via `presentation`, or visualized as UX mocks via `ux-mocks`.

### Writing & Documents
- `writing-style` - Universal writing style. Applies to all content: emails, replies, Slack, docs.
- `white-paper` - Data-driven business case for solving a customer problem.
- `vision-document` - Multi-page product vision narrative with data, tenets, and phased roadmap.
- `brd-prd` - Requirements spec for engineering with user stories, system impact, implementation logic.
- `technical-analysis` - Data source comparison, field-level mismatches, root causes, fix tracking.
- `launch-plan` - Operational readiness for high-stakes events.
- `30-60-90-day-plan` - Onboarding plan for a new role.
- `strategy-memo` - Strategy docs and one-pagers.
- `project-status` - Leadership-ready project status updates.

### Sales
- `seller-pitch` - Seller-specific pitch deck for fulfillment services or platform partnerships.

### Product & Research
- `executive` - Strategic framing, executive communication, and stakeholder alignment.
- `user-researcher` - User research analysis, pain point identification, and insight synthesis.

### Finance
- `financial-health-retirement` - Retirement readiness and financial health checkup including FIRE planning.
- `credit-card-recommender` - Credit card recommendation engine with issuer rules, offer scraping, and ranked recommendations.

### Presentation
- `presentation` - Convert markdown documents into polished slide decks via Gamma API.

### Design
- `frontend-design` - Cross-cutting aesthetic layer for all frontend work.
- `website-design` - Recreate website designs from reference screenshots using Tailwind CSS.

### UX & Prototyping
- `ux-mocks` - UX mockups and prototypes via Google Stitch AI or manual HTML/Tailwind.

### Subagents
- `code-reviewer` - Unbiased code review with zero context. Returns issues by severity with a PASS/FAIL verdict.
- `research` - Deep research via web search, file reads, and codebase exploration. Returns concise sourced findings.
- `email-classifier` - Classifies Gmail emails into Action Required, Waiting On, Reference.

### Design & Build Workflow

When building or modifying any non-trivial code (scripts, features, refactors), follow this loop:

1. **Write/edit the code** — Make your changes.
2. **Code Review** — Spawn `code-reviewer` subagent with the changed file(s). It reports issues back — it does NOT fix anything itself.
3. **Fix** — The parent agent reads the review report and applies all fixes.
4. **Ship** — Only after review passes.

**Important:** Subagents are read-only reporters. All code changes happen in the parent agent.

## Operating Principles

**1. Skills auto-activate**
Claude picks the right Skill based on your request. Each Skill's description tells Claude when to use it.

**2. Scripts are bundled**
Each Skill has its own `scripts/` folder. Run scripts from there:
```bash
python3 .claude/skills/scrape-leads/scripts/scrape_apify.py ...
```

**3. Self-anneal when things break**
- Read error message and stack trace
- Fix the script and test it again
- Update SKILL.md with what you learned
- System is now stronger

**4. Update Skills as you learn**
Skills are living documents. When you discover API constraints, better approaches, or edge cases — update the SKILL.md. But don't create new Skills without asking.

**5. Visual comparisons via surge.sh**
When a task involves comparing, critiquing, or evaluating something visual — websites, products, designs, UI patterns, before/after states — spin up a temporary surge.sh site to communicate findings visually rather than describing them in text.

## File Organization

| Folder/File | Purpose |
|---|---|
| `CLAUDE.md` | Entry point. Claude reads this automatically on every conversation. |
| `GOALS.md` | Your identity, what you own, and current goals. |
| `Tasks/` | Simple backlog → active → archive flow. |
| `Projects/` | Discrete work with its own context, research, and outputs. |
| `Workflows/` | Repeatable processes. Mini-workspaces you run again and again. |
| `Meetings/` | Notes organized by meeting type. |
| `Knowledge/` | Persistent reference material that spans projects. |
| `Templates/` | Document structures for consistent outputs. |
| `.claude/skills/` | Skills triggered by context or `/skillname`. |
| `Agents/` | Autonomous agents that run on a schedule (e.g., skill-annealer). |
| `Tools/` | Scripts, utilities, and automations. |

## Summary

You work with Skills that bundle intent (SKILL.md) with execution (scripts/). Read instructions, make decisions, run scripts, handle errors, continuously improve the system.

Be pragmatic. Be reliable. Self-anneal.
