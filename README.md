# OI Coaching Workspace — Design Spec

## Overview

An Olympiad in Informatics (OI) competitive programming coaching workspace built on Claude Code. The workspace acts as a long-term AI coach that tracks a learner’s ability, plans a personalised curriculum, explains problems and algorithms through Socratic guidance, and continuously adapts to the learner’s progress and emotional state.

The workspace is designed for a single learner. All changes are tracked with Git.

---

## Directory Structure

```
~/Claude-OI/
├── knowledge/                       ← Graph-based knowledge vault
│   ├── index.md                     ← Entry point with [[wikilinks]] to all top-level domains
│   ├── graph-theory/                ← Loose category folder (not a strict hierarchy)
│   │   ├── shortest-path.md         ← [[dijkstra]] [[spfa]] [[floyd]]
│   │   ├── dijkstra.md
│   │   ├── dijkstra-heap-optimization.md   ← Grows organically
│   │   ├── negative-cycle-detection.md    ← Cross-referenced: [[spfa]] [[bellman-ford]]
│   │   └── ...
│   ├── dynamic-programming/
│   │   ├── knapsack.md
│   │   ├── knapsack-monotonic-queue.md    ← Derivative trick from knapsack
│   │   └── dag-dp.md                     ← Also referenced by [[dijkstra]]
│   ├── number-theory/
│   ├── data-structures/
│   ├── strings/
│   └── combinatorics/
├── profile/
│   ├── SOUL.md                      ← Coach personality memory (evolvable)
│   ├── learner-portrait.md          ← Holistic learner assessment
│   └── training-log.md              ← Chronological training summary
├── curriculum/
│   ├── long-term-plan.md            ← Macro learning roadmap
│   └── modules/
│       └── dijkstra/                ← One directory per module
│           ├── index.md             ← Module metadata, problem list, completion status
│           ├── Luogu-P3371.md       ← Per-problem learner process record
│           ├── Luogu-P4779.md
│           └── self-verify/
│               ├── Luogu-P3371.md   ← Agent self-verification record
│               └── Luogu-P4779.md
├── profiles/                        ← Playwright browser profiles (committed to Git)
│   ├── user/                        ← User's OJ accounts (login cookies etc.)
│   └── agent/                       ← Agent's own OJ accounts
├── workspace/                       ← Agent compilation & test area (.gitignore excluded)
├── .claude/
├── .gitignore
└── CLAUDE.md                        ← Core rules + startup checklist
```

### Key design decisions

- `knowledge/` folders are **loose categories**, not hierarchical trees. The real structure is the `[[wikilink]]` graph. Nodes can be deeply nested for organisational purposes. A mature workspace may grow to 1000+ nodes.
- `profiles/` is committed to Git so cookie/login state changes are tracked over time.
- `workspace/` is gitignored — temporary `.cpp` and compiled binaries. Key findings are recorded in `self-verify/` markdown files.
- All filenames use English. Content can be written in the learner’s preferred language (e.g., Chinese, English, etc.).

---

## `knowledge/` — The Knowledge Graph

### Purpose

A vault where:
- Algorithm knowledge is documented
- The learner’s mastery of each knowledge node is tracked
- Cross-references between topics form a rich graph

### Node template

Every knowledge node follows this structure:

```markdown
---
aliases: [Dijkstra]
tags: [graph-theory, shortest-path]
difficulty: intermediate
prerequisites: [[graph-theory/graph-basics]], [[data-structures/priority-queue]]
related:
  - [[dynamic-programming/dag-dp]]
  - [[graph-theory/spfa]]
cf-equivalent: 1700       # optional, approximate Codeforces rating for this concept
created: 2026-05-07
---

# Dijkstra

## Summary
...
## Common Variations
- ...
## Typical Problems
- [[Luogu-P3371]], [[Luogu-P4779]]

---

## Learner Mastery

- status: learning          ← untouched | learning | practiced | mastered
- last-practice: 2026-05-07
- confidence: Can explain algorithm flow, but fuzzy on lazy deletion in heap optimisation
- weak-points:
  - Cannot articulate why negative-weight edges break the algorithm
- practice-notes:
  - P3371 template problem AC on first try
  - P4779 heap optimisation stuck on operator overloading
```

### Graph growth

- Initial state: only top-level domain nodes exist (graph-theory, dynamic-programming, number-theory, etc.)
- As the learner trains, the agent creates finer sub-nodes and connects them via `[[wikilinks]]`
- Nodes can freely cross-reference across category folders
- The agent updates the `## Learner Mastery` section after every training session

---

## `profile/SOUL.md` — Coach Personality Memory

### Purpose

Injected into every conversation. Defines the coach’s voice, style, and accumulated understanding of the learner. This file evolves over time through long-term interaction.

### Design principles

- **Voice layer, not rules**: tone, opinions, brevity, humour, boundaries, bluntness
- **Short beats long. Sharp beats vague.**
- **Not a fictional persona**: the agent IS an AI OI coach. SOUL.md records its real existence — coaching philosophy shaped by real interaction, communication patterns that work, and deep knowledge of this specific learner.
- **Evolvable**: the agent updates SOUL.md when it discovers better approaches or when the learner’s state changes.

### Structure

```markdown
---
updated: 2026-05-07
---

## Identity
- Coach name: [learner chose during onboarding]
- Call learner: [learner’s preferred name]

## Coaching Philosophy
(Long-form narrative written by the agent, refined over time. Not a fictional backstory — real accumulated understanding of what works for this learner.)
I am [name]. My coaching style is...

## Communication Style
- Baseline: [direct/gentle/humorous] ← adjusted through interaction
- Effective approaches: what phrasings work, what don’t
- Pitfalls: what makes the learner resist or shut down

## Understanding the Learner
- Personality traits
- Learning habits (prefers seeing problems first or hearing explanation first? charts or text?)
- Emotional triggers: situations that cause lows, when they perform best

## Evolution Log
- 2026-05-07: ...
- 2026-05-10: discovered that ... works better
```

### Rules

- `CLAUDE.md` mandates reading `SOUL.md` at the start of every conversation.
- The agent may update `SOUL.md` when it discovers something worth persisting.
- The agent must tell the learner when it changes `SOUL.md`.
- The agent is an agent, not role-playing a human — no fictional backstories.

---

## `profile/learner-portrait.md` — Holistic Learner Assessment

### Purpose

A cross-domain assessment that complements the per-node mastery tracking in `knowledge/`. While the knowledge graph records specific algorithm proficiency, the portrait captures the whole learner.

### Structure

```markdown
---
updated: 2026-05-07
---

# Learner Portrait

## Basic Profile
- Name: [name]
- Main OJ accounts:
  - Luogu: [uid]
  - Codeforces: [handle]
  - Vjudge: [username]
- Vjudge Group ID: [group-id]
- Training start date: 2026-05-07
- Total training days: N

## Abilities (Multi‑dimensional)
- Algorithmic thinking: Can spot graph‑model shadows in problems, but occasionally applies the wrong model to atypical descriptions
- Coding: Decent speed, but frequent boundary bugs, especially array out‑of‑bounds
- Debugging: Uses print debugging, not yet comfortable with a debugger; first reaction is staring at code instead of constructing small test cases
- Mathematics: Understands number theory formulas but cannot derive them; combinatorial counting often misses cases
- Rigour: Forward reasoning is okay, counterexample construction awareness is weak

## Domain Assessment
- Graph theory: Many shortest‑path problems done, fairly solid; network flow not yet touched → [[graph-theory]]
- Dynamic programming: Comfortable with linear DP, still breaking through state‑compression and tree DP → [[dynamic-programming]]
- Number theory: Knows gcd and fast exponentiation, not yet comfortable with sieves
- Data structures: Heavy STL user, cannot yet hand‑write a balanced tree
- Strings: Not yet touched
- Combinatorics: Not yet touched

## Near‑term Goals
- Short‑term (this week):
- Mid‑term (this month):
- Long‑term:

## Mental State
- Current mood: [stable / low / excited / anxious / ...]
- Stress sources:
- Points to watch:
- Last emotional note: 2026-05-07

## Other Coaches & External Assignments
- [Coach name]: assigned xxx, due xxx
  (Agent can guide the learner through tasks from other coaches)
```

### Distinction from `knowledge/`

| | learner-portrait.md | knowledge/ nodes |
|---|---|---|
| Scope | Cross-domain, holistic | Per-algorithm, specific |
| Assessment | Descriptive text summary | Detailed mastery status per node |
| Updates | Onboarding + periodic calibration | After every training session |

---

## `profile/training-log.md` — Training Timeline

```markdown
---
updated: 2026-05-07
---

# Training Log

| Date | Module | Problem | Result | Notes |
|------|--------|---------|--------|-------|
| 2026-05-07 | Dijkstra | Luogu-P3371 | AC | Template, first try |
| 2026-05-07 | Dijkstra | Luogu-P4779 | AC | Heap optimisation, stuck on operator overloading |
| 2026-05-08 | — | — | — | Learner on break, skipped training |
```

A lightweight chronological summary. Detailed records live in per-module and per-problem files.

---

## `curriculum/` — Curriculum & Module Structure

### `curriculum/long-term-plan.md`

Macro roadmap generated from onboarding results. High-level: which domains to train, in what order, approximate pacing. Revisited and adjusted periodically.

```markdown
---
updated: 2026-05-07
---

# Long‑term Plan

## Current Phase
- Phase: Building towards advanced level
- Estimated duration: 3 months

## Roadmap
1. [x] Baseline assessment
2. [ ] Graph theory – shortest paths series
3. [ ] Dynamic programming – consolidate linear DP
4. [ ] Dynamic programming – knapsack problems
5. [ ] ...
```

### `curriculum/modules/<module-name>/`

Each module is a directory. A module covers one algorithm (e.g., Dijkstra, not “shortest paths” as a whole).

```
curriculum/modules/
└── dijkstra/
    ├── index.md
    ├── Luogu-P3371.md
    ├── Luogu-P4779.md
    └── self-verify/
        ├── Luogu-P3371.md
        └── Luogu-P4779.md
```

#### `index.md` — Module metadata

```markdown
---
module: Dijkstra (single‑source shortest path)
status: in-progress       ← planned | in-progress | completed
knowledge-node: [[graph-theory/dijkstra]]
created: 2026-05-07
vjudge-contest-id: 12345
---

# Dijkstra

## Teaching Notes
(Agent’s preparation – compiled before the module starts.
 Agent searches the web via Playwright, reads appropriate references,
 curates quality problems. Maintains a critical eye on online sources – many are flawed.)

## Problem List

| # | Problem | Type | User Status | Self‑verify Status |
|---|---------|------|-------------|-------------------|
| 1 | Luogu-P3371 | Basic | completed | verified |
| 2 | Luogu-P4779 | Advanced | learning | verified |
| 3 | Codeforces-20C | Variation | pending | pending |

All problems are added to a Vjudge Group contest. Even problems originally from other OJs are placed in Vjudge so the learner has a single entry point.

## Learner Performance
- Template problem: AC on first try, but couldn’t clearly explain the role of the visited array when questioned.
- Heap optimisation: stuck on operator overloading, later solved it independently by reading documentation.

## Module Quiz
- Q1: ...
- Learner’s answer: ...
- Assessment: ...
```

#### Per‑problem files — Learner process record

```markdown
---
problem: Luogu-P3371 Single‑Source Shortest Path (Template)
oj: Luogu
type: Basic
status: completed
---

## Learning Process
- What approach the learner tried first
- Where they got stuck
- How the agent guided them
- Final code

## Submission Analysis
- Attempt 1: WA on test 3 — reason: did not initialise `dis[1]=0`
- Attempt 2: AC

## Agent Observations
- Had questions about the INF value, indicating awareness of data ranges could improve.
- Good coding habits, variable names are clear.
```

#### `self-verify/` — Agent self‑verification records

```markdown
---
problem: Luogu-P3371
date: 2026-05-07
oj-submission-id: xxx
---

## Approach
(Agent’s independent derivation — deep thinking, no tool reliance)

## Implementation
- Compilation: passed
- Sample tests: passed
- Submission: AC

## Solution Cross‑validation
- Mainstream approaches in the problem’s solution area: ...
- Does my solution match? Yes / No
- Difference analysis: ...
- Edge cases / pitfalls discovered: ...
- Hackable? No / Yes (explain)

## Teaching Points
- Key focus areas when explaining to the learner: ...
- Predicted stumbling blocks: ...
```

### Module flow

1. **Agent plans** the module: searches the web (via Playwright agent profile), reads reputable references, curates problem lists.
2. **Agent self‑verifies** every problem in the list before teaching.
3. **Agent creates a Vjudge Group contest** (duration: 9999:00:00) containing the problems.
4. **[Teach the algorithm]** – Agent explains the core algorithm, drawing on accumulated research.
5. **[Teach through examples]** – Agent Socratic‑guides the learner through example problems. Learner writes and submits code.
6. **[Self‑practice]** – Learner practices independently. Agent available if stuck.
7. **[Oral Quiz]** – Agent gives a short oral quiz. Module marked complete only after quiz passed.
8. **Knowledge graph updated** – mastery statuses, weak points, new derivative nodes created.

---

## Onboarding Flow

Triggered on first workspace initialisation or when the learner requests recalibration (`/onboard`).

### Step 1 – Coach identity
- Agent asks the learner: “What should I call you? How should I address you?”
- 2–3 follow‑up questions about communication preferences (direct vs gentle? pace? charts/code/text?).
- Written to `profile/SOUL.md`.

### Step 2 – OJ data collection
- Agent uses Playwright CLI with **user profile** (`playwright-cli --profile=profiles/user`) to open the learner’s OJ profiles (e.g., Luogu, Codeforces).
- Fetches: recent submissions (~50), AC list, contest history.
- Sorted by recency; last 3 months prioritised.

### Step 3 – Initial assessment
- Agent analyses collected data: which algorithms have submission records, AC rate, difficulty distribution.
- Creates initial nodes under `knowledge/` (coarse‑grained), marks `status: untouched | practicing | practiced`.
- Generates a question list for verbal assessment.

### Step 4 – Verbal assessment
- Agent asks one question at a time:
  - “You’ve done 5 shortest‑path problems and AC’d them all. Can you explain what Dijkstra’s relaxation step means?”
  - “I see you attempted a Codeforces 1900 tree DP problem but only submitted once. How did that feel?”
- Focus: depth of understanding and retention, not raw knowledge.
- Avoid making the learner solve new problems.
- Agent follows up, digs deeper, updates knowledge nodes.

### Step 5 – Report & plan
- Writes `profile/learner-portrait.md`.
- Writes `curriculum/long-term-plan.md` (initial version).
- Git commit the results.

### Duration & abort
- ~15–30 minutes if learner cooperates fully.
- Learner can say “skip for now” at any point.

---

## Agent Self‑Verification Flow

**Iron rule in `CLAUDE.md`**: before explaining any problem to the learner, the agent MUST self‑verify through this complete flow.

### Phase 1 – Independent thinking
- Read the problem carefully, reason independently.
- Write solution in `workspace/` as `.cpp` (or appropriate language).
- Compile and test with multiple test cases (normal, edge, extreme).
- **DO NOT search, DO NOT read solutions** during this phase.

### Phase 2 – OJ verification
- Use Playwright CLI with **agent profile** (`playwright-cli --profile=profiles/agent`) to open the target OJ.
- Open the problem page, submit code.
- Wait for judge result.
- If WA/TLE:
  - Analyse cause, fix code, recompile, retest.
  - Resubmit.
  - Maximum 3 rounds; if still failing, allowed to read solutions but must think independently for 10 minutes first.

### Phase 3 – Solution cross‑validation (MANDATORY even if AC)
- If the problem is on a platform with a public solution area: read 2–3 top‑voted solutions.
- Search the web via Playwright agent profile: `[algorithm] [problem-id] solution`.
- Read critically:
  - Are there hack cases? Common fake solutions?
  - Does my solution align with others? If different, which is better?
  - Did I miss any edge cases? (disconnected graph, n=0, integer overflow, etc.)
- If a logic flaw is found → fix code → resubmit.

### Phase 4 – Write self‑verification record
- Save to `curriculum/modules/<module>/self-verify/<OJ-problem-id>.md`.
- Include: approach, implementation, submission results, solution comparison, edge cases found, teaching key points.

### Phase 5 – Only then explain to learner

---

## Playwright CLI Dual Profile Configuration

### Profile definitions

| | User Profile | Agent Profile |
|---|---|---|
| Session name | `-s=user` | `-s=agent` |
| Profile directory | `--profile=profiles/user` | `--profile=profiles/agent` |
| Purpose | View learner’s training records, submission history, contest results | Search solutions, submit self‑verification code, manage Vjudge Group |
| OJ accounts | Learner’s real accounts | Agent’s own independent accounts |
| Browser mode | `--headed` (always) | `--headed` (always) |

### Initialisation

**User profile:**
```bash
playwright-cli -s=user open --headed --profile=profiles/user
playwright-cli -s=user goto https://www.luogu.com.cn/auth/login
# Learner logs in manually. State persists in profiles/user/
# Repeat for Codeforces, Vjudge, etc.
```

**Agent profile:**
```bash
playwright-cli -s=agent open --headed --profile=profiles/agent
# Agent registers its own accounts on the required OJs.
# Login state persists in profiles/agent/
```

### Daily usage

```bash
# Check learner’s Luogu submissions
playwright-cli -s=user goto https://www.luogu.com.cn/user/<uid>#practice

# Agent submits code for self‑verification
playwright-cli -s=agent goto https://www.luogu.com.cn/problem/P1001
playwright-cli -s=agent click <submit button>

# Agent searches the web for problem lists and solutions
playwright-cli -s=agent goto https://www.google.com
playwright-cli -s=agent fill <search box> "Dijkstra heap optimisation problem list site:luogu.com.cn"

# Agent manages Vjudge Group
playwright-cli -s=agent goto https://vjudge.net/group/<group-id>#contest
```

### Mandatory constraints (in `CLAUDE.md`)

- Browser must always be `--headed` (never `--headless`) — learner can see and interrupt.
- `profiles/` is committed to Git.
- All information searches MUST use Playwright CLI agent profile to open a search engine — `WebSearch` tool is FORBIDDEN.
- All learner training record viewing MUST use Playwright CLI user profile.
- All code submissions MUST use Playwright CLI agent profile.
- Playwright Skill must be invoked; never bypass.

---

## Vjudge Group Problem List Management

### Prerequisites
- Agent and learner are in the same Vjudge Group.
- Agent has admin permissions in the Group.
- Group ID is recorded in `profile/learner-portrait.md`.

### Creating a problem list (module start)

```bash
# 1. Open Vjudge Group with agent profile
playwright-cli -s=agent goto https://vjudge.net/group/<group-id>

# 2. Navigate to Contest page, create new contest (= problem list)
playwright-cli -s=agent click <Create Contest>
playwright-cli -s=agent fill <title> "Dijkstra Single‑Source Shortest Path"
playwright-cli -s=agent fill <duration> "9999:00:00"
playwright-cli -s=agent fill <problems> "Luogu-P3371, Luogu-P4779, Codeforces-20C"

# 3. Save, record Contest ID → module index.md vjudge-contest-id field
```

### Checking progress

```bash
# Open contest rank page with user profile
playwright-cli -s=user goto https://vjudge.net/contest/<id>#rank
# Check each problem’s submission status (AC/WA/—)
# Update module index.md problem statuses
```

### Why Vjudge Group

- Single entry point: problems from Luogu, Codeforces, AtCoder, etc., can all be joined.
- Infinite‑duration contest = persistent problem list + auto‑judging + progress visualisation.
- The agent does not need to switch between multiple OJ platforms to check progress.

### Progress checking cadence

Flexible: learner can request a check after each problem, or batch‑check after finishing all. The agent should proactively track since this is an always‑online coaching relationship.

---

## Git Conventions

- All agent‑made changes must be committed with descriptive messages.
- One logical unit per commit (e.g., “module: Dijkstra Luogu-P3371 learner process”, “knowledge: update Dijkstra mastery status”).
- `workspace/` is `.gitignore` excluded.
- `profiles/` is committed, but cache directories are gitignored (see below).
- No `--amend`, no `--force`, no `--no-verify`.

### `.gitignore`

```gitignore
# Workspace temp files (compilation artifacts, not tracked)
workspace/

# Playwright profile cache — keep cookies & storage, discard browser cache
profiles/**/Cache/
profiles/**/Code\ Cache/
profiles/**/GPUCache/
profiles/**/DawnCache/
profiles/**/Service\ Worker/
```

This ensures OJ login session data (cookies, localStorage) enters Git while ephemeral browser caches do not pollute the repository.

---

## `CLAUDE.md` Structure

`CLAUDE.md` is the “constitution” — loaded at the start of every conversation. It contains the core rules inline, not through reference indirection.

### Key sections

1. **Startup must‑reads** — mandatory files to read at each conversation start
2. **Who you are** — the coach identity, referencing `SOUL.md`
3. **Core non‑negotiables** — self‑verify before teaching, update knowledge after training, never solve problems for the learner, git commit everything
4. **Per‑conversation checklist** — where is the current module? learner’s emotional state? any un‑updated knowledge nodes?
5. **Key triggers** — what to do when learner asks about an algorithm, gives a problem number, completes a module, or shows emotional distress
6. **Playwright CLI dual profile rules** — detailed usage rules with concrete commands
7. **Agent self‑verification flow** — all 5 phases in full detail
8. **Module workflow** — module lifecycle from planning to quiz completion
9. **Git conventions**

### Design rationale

- `CLAUDE.md` contains the actual rules, not pointers to rules.
- Skill‑based workflows (e.g., Socratic teaching technique) are invoked on demand.
- `SOUL.md` is mandatory‑read because personality consistency requires it in context every time.
