# ProFlow

> From vague requirement to working POC — in 2 days, with 1 developer, using 5 structured prompts.



## The Problem with "Just Vibe Code It"

Most AI-assisted development goes: **requirement → code**.

That's why it fails on anything non-trivial. The AI produces technically correct code that solves the wrong problem — because no one modelled the domain first.

ProFlow inserts the two steps everyone skips:

```
Requirement → Business Domain Model → DDD Mapping → Spec → Code
```

The result: a POC that reflects how the business actually works, not just what the manager typed into a chat window.

**Validated across 5 POCs. Token budget: 600–800K per POC. Time: 2 days.**


## How It Works

ProFlow is a 5-step prompt chain. Each step produces a structured output that feeds directly into the next. No code until Step 5.

```
┌─────────────────────────────────────────────────────────────┐
│  Step 1 │  Requirement Intake                               │
│         │  Raw manager requirement → structured problem     │
│         │  statement                                        │
├─────────────────────────────────────────────────────────────┤
│  Step 2 │  Persona & Business Process Model       ★         │
│         │  Core application persona explains the            │
│         │  entire domain in plain English.                  │
│         │  No tech jargon. This is the problem-solver       │
│         │  step — the most important in the entire flow.    │
├─────────────────────────────────────────────────────────────┤
│  Step 3 │  DDD Domain–Subdomain Quadrant                    │
│         │  Business knowledge → bounded contexts →          │
│         │  core / supporting / generic subdomains           │
├─────────────────────────────────────────────────────────────┤
│  Step 4 │  Spec Document Generation                         │
│         │  Production-ready spec formatted for              │
│         │  Claude Code, Codex, or Cursor                    │
├─────────────────────────────────────────────────────────────┤
│  Step 5 │  Automated Scaffolding via SKILL.md               │
│         │  Frontend + backend built without wasting         │
│         │  tokens or repeating context                      │
└─────────────────────────────────────────────────────────────┘
```

### Why Step 2 is the most important step

Every other "requirements to code" framework skips the domain knowledge step. They go straight from what the manager said to what the system should do. The gap between those two things is where every failed project lives.

Step 2 forces you to articulate the application through the lens of its core persona — the person or system that *owns* the problem. The output is plain English: no entities, no endpoints, no database tables. Just a clear explanation of what the domain is and how it works.

Once that exists, everything downstream — the DDD mapping, the spec, the code — is structurally correct because it's grounded in real domain understanding.

## The SKILL.md System

ProFlow uses SKILL.md files to automate the scaffolding step without burning tokens on repeated context-setting.

Each SKILL.md file is a structured instruction set that tells the AI model exactly how to build within a specific layer — conventions, patterns, constraints, and decisions already made.

| File | Purpose |
|------|---------|
| `skills/frontend.md` | Component structure, state management, routing conventions |
| `skills/backend.md` | API design, service layer patterns, data access conventions |
| `skills/domain_knowledge.md` | DDD concepts + project-specific domain-subdomain quadrant — carries domain knowledge from Step 3 into Step 5 so scaffolding is domain-aware, not just technically correct|

The model reads the relevant SKILL.md at the start of each scaffolding session. No repeated explanation. No drift from conventions. Token-efficient by design.

---

## Efficiency

| Metric | Value |
|--------|-------|
| Developers needed | 1 |
| Time to working POC | 2 days |
| Token budget per POC | 600–800K |
| Steps | 5 |
| Validated POCs | 5 |

600–800K tokens over 2 days sounds like a lot. In practice, it's extremely efficient for a full working POC with a correctly modelled domain. Most ad-hoc AI-assisted development burns more tokens on backtracking and correction than ProFlow uses end-to-end.

---

## Validated POCs
 
ProFlow has been tested against 5 different POCs across different domains:
 
| # | Domain | Outcome |
|---|--------|---------|
| 1 | Search & recommendation | Semantic search with personalised recommendations — domain model defined retrieval boundaries before any vector DB decisions |
| 2 | API integration layer | Low-intervention connector for rapid external resource onboarding — spec surfaced integration edge cases before a single endpoint was designed |
| 3 | Procurement automation | Procurement workflow with AI-driven email outreach — persona model caught business rules that the original requirements had missed entirely |
| 4 | Data acquisition pipeline | Automated web sourcing pipeline — DDD quadrant separated core scraping logic from downstream processing before build began |
| 5 | Productivity tool integration | Surfaced genuine technical constraints at spec stage, not after weeks of development — early invalidation is a valid and valuable POC outcome |
 
POC domains are intentionally generalised. The prompts and SKILL.md files are the transferable layer — not the specific business context they were applied to.

## Prerequisites

- Claude (Sonnet or Opus), GPT-4o, or any capable frontier model
- Claude Code, Cursor, or Codex for Step 5 scaffolding
- Familiarity with Domain Driven Design concepts helps but is not required — Step 3 explains itself

---

## Usage

**Step 1 — Intake**

Open `prompts/01-requirement-intake.md`. Paste your requirements. Run it. You get a structured problem statement.

**Step 2 — Persona Model**

Open `prompts/02-persona-business-process.md`. Feed in the Step 1 output. This is the step to spend time on — review the output carefully, push back on anything that doesn't reflect how the business actually works.

**Step 3 — DDD Mapping**

Open `prompts/03-ddd-domain-quadrant.md`. Feed in the Step 2 output. You get a domain–subdomain quadrant with core, supporting, and generic subdomains identified.

**Step 4 — Spec Generation**

Open `prompts/04-spec-generation.md`. Feed in Steps 2 + 3. You get a spec document ready to feed into Claude Code, Codex, or Cursor.

**Step 5 — Scaffold**

Open Claude Code (or your preferred tool). Load the relevant SKILL.md files. Feed in the Step 4 spec. Build.

---

## Repository Structure

```
proflow/
├── README.md
├── prompts/
│   ├── 01-requirement-intake.md
│   ├── 02-persona-business-process.md
│   ├── 03-ddd-domain-quadrant.md
│   ├── 04-spec-generation.md
│   └── 05-scaffold-automation.md
├── skills/
│   ├── frontend.md
│   ├── backend.md
│   └── domain_knowledge.md
```


## Who This Is For

- **Solo developers** who need to move from whiteboard to working software fast
- **AI leads** who need a repeatable process to hand to junior developers
- **Founders** who want to validate product ideas without a full team
- **Anyone** tired of AI-generated code that's technically correct but domain-wrong



## Why Not Just Use an Agent?

Agents are good at execution. They're bad at understanding. An agent given a vague requirement will build confidently in the wrong direction.

ProFlow is not an agent. It's a structured thinking process that happens to be assisted by AI. The human stays in the loop at every step — especially Step 2, where domain understanding either gets captured correctly or doesn't. The agent (Claude Code, Codex, Cursor) only appears at Step 5, when there's enough structure for it to execute reliably.


> use the chat interface of claude or chatgpt. not IDE before coding till step 3. A little tip of personal experience


## Contributing

If you've used ProFlow to build a POC, open a PR adding your example to `/examples`. Include the Step 2 and Step 3 outputs — those are the most useful artifacts for others to learn from.

