
# index.md
# KaalAman — Foundational Matrix Document Index

## Purpose

This is the **master pointer file** for the KaalAman project. It tells humans and AI agents which document to consult, when to use it, and what each one contains.

**This file is a POINTER. It contains NO full content.** If you need details, follow the pointer to the specific document. Never load all documents at once — that wastes tokens and causes context rot.

---

## Quick Reference

| I need to know... | Read this → | Section hint |
|-------------------|-------------|-------------|
| How the coding agent should behave | `agents.md` | Start here for all agent behavior rules |
| What we're building and what's in/out of scope | `prd.md` | § Feature Matrix for MUST/SHOULD/WON'T |
| How the system architecture works | `system-design.md` | § Component Diagram, § Tier Detection |
| Why a feature exists + acceptance criteria | `user-stories.md` | Find by Epic (1-8) or Story ID (US-X.X) |
| Colors, fonts, spacing, components, voice | `brand-guidelines.md` | § 3 Colors, § 4 Typography, § 11 Tailwind Config |
| Every page, wireframe, navigation flow | `sitemap.md` | § 2 Page Specs, § 3 Navigation Flows, § 4 Component Map |
| What to build next (phase order) | `agents.md` | § 6 Feature Implementation Order |
| Prompt templates for AI features | `agents.md` | § 7 Prompt Engineering |
| BKT adaptive learning math | `agents.md` | § 8 BKT Implementation |
| File/folder structure | `agents.md` | § 9 File & Folder Structure |
| Git workflow and commit conventions | `agents.md` | § 11 Git Workflow |
| Pre-submission testing checklist | `agents.md` | § 12 Testing Checklist |
| Demo rehearsal script (7 steps) | `user-stories.md` | § Acceptance Test Script — Demo Night |
| Survey data and student quotes | `research/survey-data.csv` | Raw data (n=40 Filipino students) |
| Big 4 + MBB consulting evidence | `research/consulting-evidence.md` | McKinsey, BCG, PwC, EY, Deloitte findings |
| Competitive landscape | `prd.md` | § Competitive Analysis |
| Pricing and market size | `prd.md` | § Market Opportunity |

---

## Document Registry

### 1. `agents.md` — Agent Behavior Instructions
- **Authority:** How the coding agent should behave, code style, build order
- **Contains:** Project identity, tech stack rules, tier detection logic, implementation phases, prompt templates, BKT engine, file structure, git workflow, testing checklist, emergency protocols, Kiro configuration, context rot prevention
- **When to read:** FIRST document any agent should read. Read once at session start, then reference specific sections as needed.
- **Size:** ~4,000 words
- **Version:** 3.0 (FINAL)

### 2. `prd.md` — Product Requirements Document
- **Authority:** What to build, feature scope, MVP boundaries, what's in/out
- **Contains:** Problem statement (with survey + consulting evidence), solution overview, tri-modal architecture, feature matrix (MUST/SHOULD/WON'T), competitive analysis, market opportunity, pricing model, success metrics, judging criteria alignment
- **When to read:** When deciding whether to build a feature, when checking scope boundaries, when preparing the pitch
- **Size:** ~3,500 words
- **Version:** 3.0 (FINAL)

### 3. `system-design.md` — System Design Document
- **Authority:** How the system works, architecture, data flow, API contracts
- **Contains:** Component diagram, tier detection flowchart, data flow (upload → embed → store → retrieve → generate), IndexedDB schema (all stores), API Gateway + Lambda contract, Bedrock request/response format, ONNX embedding pipeline, HNSW vector search config, PWA service worker strategy, performance budgets
- **When to read:** When implementing a service, when debugging data flow, when setting up infrastructure
- **Size:** ~4,000 words
- **Version:** 3.0 (FINAL)

### 4. `user-stories.md` — User Stories
- **Authority:** Why features exist, acceptance criteria, test conditions
- **Contains:** 26 user stories across 8 epics, each with: story statement, real student quotes (from survey), survey evidence, acceptance criteria (checkboxes), tier behavior table, priority level. Also includes: story map (build priority matrix), story-to-feature traceability table, 7-step acceptance test script for demo night
- **When to read:** When implementing a specific feature (find by Story ID), when writing tests, when rehearsing the demo
- **Size:** ~5,000 words
- **Version:** 3.0 (FINAL)

### 5. `brand-guidelines.md` — Brand & Design System
- **Authority:** How it looks, how it sounds, how it feels
- **Contains:** Brand identity (name, tagline, logo), target persona ("Maria"), color system (primary, tier indicators, semantic, mastery, neutrals), typography (Inter, scale, Tailwind config), spacing & layout (4px base, mobile-first grid), component library (buttons, cards, tier badges, quiz cards, chat bubbles, mastery bars, progress bars, toasts, empty states), iconography (Heroicons map), voice & tone (writing rules, tier naming, error messages, celebration messages), anti-patterns (what we will NEVER build), accessibility standards, complete Tailwind config, PWA manifest
- **When to read:** When building any UI component, when writing user-facing text, when choosing colors/fonts/spacing
- **Size:** ~5,500 words
- **Version:** 3.0 (FINAL)

### 6. `sitemap.md` — Page Hierarchy, Navigation & Component Map
- **Authority:** Every page, wireframe, navigation path, and component placement
- **Contains:** Full page hierarchy (bird's-eye view), detailed wireframes for all 3 pages (Home, Document, Settings) + 3 tabs (Summary, Quiz, Chat) + onboarding overlay, quiz state machine, chat empty/active states, tier-specific rendering tables, navigation flow maps (happy path, returning user, offline journey, upload journey), component-to-page mapping (24 components), React Router v6 route config, IndexedDB read/write per page, responsive behavior rules, page-to-user-story cross-reference
- **When to read:** When building any page or component, when wiring navigation, when deciding what goes where on screen, when planning the demo flow
- **Size:** ~5,000 words
- **Version:** 3.0 (FINAL)

### 7. `research/survey-data.csv` — Raw Survey Data
- **Authority:** Primary user research (ground truth)
- **Contains:** 40 responses from Filipino college students (Oct 2026), 19 questions covering: education context, device/internet reality, study friction frequency, peer lockout estimates, budget sensitivity, annoyance rankings, pain stories, workarounds, tool preferences, god-tier features, bloatware complaints, setup tolerance, cloud vs. local preference, willingness to pay, price range, beta interest
- **When to read:** When you need a specific data point or student quote to validate a decision
- **Size:** ~40 rows × 19 columns

### 8. `research/consulting-evidence.md` — Validation Evidence
- **Authority:** External validation from consulting firms and development data
- **Contains:** McKinsey (AI tutoring ROI, learning gains), BCG (Rajasthan case study — 400K students), PwC (offline-first as adoption enabler), EY (SLMs for infrastructure-constrained markets), Deloitte (EdTech market projections), World Bank (learning poverty data, meta-analysis), Kenya edge-AI deployment results (31% STEM gains), TinyML compression benchmarks, Philippines PISA 2025 scores, SE Asia EdTech market sizing ($41.52B by 2034)
- **When to read:** When preparing the pitch, when a judge asks "where's the evidence?", when writing the README
- **Size:** ~3,000 words

---

## Document Relationships

```
                    ┌─────────────┐
                    │  index.md   │ ← YOU ARE HERE
                    │  (pointer)  │    Points to everything.
                    └──────┬──────┘    Contains nothing.
                           │
           ┌───────────────┼───────────────┐
           │               │               │
    ┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐
    │  agents.md  │ │   prd.md    │ │  research/  │
    │  (behavior) │ │   (scope)   │ │  (evidence) │
    └──────┬──────┘ └──────┬──────┘ └─────────────┘
           │               │
    ┌──────┴───────────────┴──────┐
    │                             │
    │  These five inform each     │
    │  other but don't duplicate  │
    │                             │
    ├───────┬───────┬──────┬──────┤
    │       │       │      │      │
┌───▼──┐┌──▼───┐┌──▼──┐┌──▼──┐┌──▼──┐
│system││ user ││brand││site-││     │
│design││stori-││guide││ map ││     │
│ .md  ││es.md ││.md  ││ .md ││     │
│(arch)││(why) ││(look)││(nav)││     │
└──────┘└──────┘└─────┘└─────┘└─────┘
```

**Flow:**
1. `agents.md` tells the agent HOW to behave and WHERE to look
2. `prd.md` tells the agent WHAT to build
3. `system-design.md` tells the agent HOW the system works
4. `user-stories.md` tells the agent WHY each feature exists and HOW to test it
5. `brand-guidelines.md` tells the agent HOW it should look and sound
6. `sitemap.md` tells the agent WHERE everything goes on screen and HOW pages connect
7. `research/` provides the EVIDENCE that validates everything above

---

## Authority Hierarchy

When two documents conflict, the one higher in this list wins **for its domain:**

| Rank | Document | Wins for... |
|------|----------|-------------|
| 1 | `agents.md` | Agent behavior, code style, build order, tech stack rules |
| 2 | `prd.md` | Feature scope, what's in/out, MVP boundaries |
| 3 | `system-design.md` | Architecture, data flow, API contracts, schemas |
| 4 | `user-stories.md` | Acceptance criteria, test conditions, user needs |
| 5 | `brand-guidelines.md` | Colors, fonts, spacing, components, voice, tone |
| 6 | `sitemap.md` | Page hierarchy, wireframes, navigation flows, component placement |
| 7 | `research/` | Data points, statistics, quotes, market numbers |

**Example:** If `agents.md` says "use React Context" but `system-design.md` says "use Redux," `agents.md` wins because tech stack is its domain. If `agents.md` mentions a color but `brand-guidelines.md` specifies a different hex value, `brand-guidelines.md` wins because colors are its domain. If `brand-guidelines.md` shows a component wireframe but `sitemap.md` places it on a different page, `sitemap.md` wins because page placement is its domain.

---

## Token Budget Guide

For AI agents with limited context windows, here's how to manage token consumption:

| Scenario | What to load | Estimated tokens |
|----------|-------------|-----------------|
| **Starting a new session** | `index.md` (this file) + `agents.md` | ~5,000 |
| **Building a UI component** | `brand-guidelines.md` § relevant component + `sitemap.md` § relevant page | ~1,200-1,800 |
| **Wiring navigation / routing** | `sitemap.md` § 3 Navigation Flows + § 5 Route Config | ~800-1,000 |
| **Implementing a feature** | `agents.md` § relevant phase + `user-stories.md` § relevant story | ~600-1,000 |
| **Debugging architecture** | `system-design.md` § relevant section | ~500-800 |
| **Checking scope** | `prd.md` § Feature Matrix | ~400 |
| **Writing user-facing text** | `brand-guidelines.md` § 8 Voice & Tone | ~500 |
| **Preparing the pitch** | `prd.md` + `research/consulting-evidence.md` | ~6,500 |

**Rules:**
- NEVER load all 8 documents at once (~30,000+ tokens = context rot guaranteed)
- Load `index.md` + `agents.md` at session start, then load others ONE AT A TIME
- Extract only the specific SECTION you need, not the entire document
- When done with a document, mentally "unload" it — don't keep referencing stale context

---

## For Teammates (Humans)

If you're a teammate joining the project mid-build, here's your onboarding path:

### 30-Second Orientation
1. **Read this file** (`index.md`) — you're doing it now ✓
2. **Read `agents.md` § 1-3** — project identity, document references, core behavior rules
3. **Check `agents.md` § 6** — find which PHASE the team is currently in
4. **Read the last 3 git commit messages** — understand what was just built
5. **Start coding** — reference specific FMDs as needed

### Role-Based Reading

| Your role tonight | Read these first |
|-------------------|-----------------|
| **Frontend dev** | `sitemap.md` (full — your bible), `brand-guidelines.md` (full), `agents.md` § 9 (file structure) |
| **Backend/infra dev** | `system-design.md` (full), `agents.md` § 4.4 (backend rules), `agents.md` § 7 (prompts) |
| **AI/ML dev** | `agents.md` § 7-8 (prompts + BKT), `system-design.md` § AI pipeline, `user-stories.md` Epic 2-4 |
| **Pitcher/presenter** | `prd.md` (full), `research/consulting-evidence.md` (full), `user-stories.md` § Demo Script, `sitemap.md` § 3.3 (offline journey for demo) |
| **QA/testing** | `user-stories.md` (all acceptance criteria), `agents.md` § 12 (testing checklist), `sitemap.md` § 3 (all navigation flows) |

---

## Version Control

| Document | Version | Status | Last Updated |
|----------|---------|--------|-------------|
| `index.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `agents.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `prd.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `system-design.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `user-stories.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `brand-guidelines.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `sitemap.md` | 3.0 | FINAL — Locked for Build | Oct 3, 2026 |
| `research/survey-data.csv` | 1.0 | FINAL | Oct 2, 2026 |
| `research/consulting-evidence.md` | 2.0 | FINAL | Oct 3, 2026 |

**All documents are v3.0. There are 9 entries total (7 FMDs + 2 research files). There is no v1 or v2 in this repository.** If you encounter references to earlier versions anywhere, they are outdated. Only the documents listed above are authoritative.

---

## Kiro Integration

When Kiro reads this file, it should:

1. **Register this as the document map** — know where to find everything
2. **Load `agents.md` next** — get behavior rules and build order
3. **Never load more than 2 FMDs simultaneously** — token budget discipline
4. **Follow the authority hierarchy** — when documents conflict, defer to the domain owner
5. **Check the phase in `agents.md` § 6** — know what to build next

### `.kiro/settings.json` should point here:

```json
{
  "agent": {
    "instructionsFile": "docs/agents.md",
    "contextFiles": ["docs/index.md"]
  }
}
```

This ensures Kiro loads `agents.md` as its primary instruction set and `index.md` as its navigation map. All other documents (including `sitemap.md`) are loaded on-demand.

---

## The One Rule

> **If it's not in a document, it doesn't exist.**
>
> Verbal agreements, Slack messages, and "I thought we agreed" don't count.
> If a decision matters, it lives in an FMD. If it's not in an FMD, it hasn't been decided.
>
> — Adapted from the hackathon strategies talk: *"If an important decision or piece of work does not live in shared documents, the team cannot reliably act as though it happened."*
