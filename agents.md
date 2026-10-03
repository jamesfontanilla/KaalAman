
# agents.md
# KaalAman — Coding Agent Behavior Instructions

## Purpose

This file defines how the coding agent (Kiro, Cursor, or any AI IDE) should behave when working on the KaalAman codebase. It is the SINGLE SOURCE OF TRUTH for agent behavior. All tool-specific config files (e.g., `.kiro/`, `.cursorrules`) should redirect here.

**Do NOT put the full document index in this file.** Consult `index.md` for the complete list of foundational documents and when to reference each one. This file focuses ONLY on agent behavior rules.

---

## 1. Project Identity

| Field | Value |
|-------|-------|
| **Project** | KaalAman — Offline-First Edge-AI Study Companion |
| **Tagline** | "Your notes. Your device. Your pace." |
| **What it does** | Converts uploaded PDFs into summaries, adaptive quizzes, and RAG-powered chat — works offline |
| **Target user** | Filipino college students on budget devices with unreliable internet |
| **Tech stack** | React + Vite PWA, Tailwind CSS, IndexedDB, ONNX Runtime (MiniLM), llama.cpp WASM (SmolLM2), AWS Lambda + API Gateway + Bedrock (Claude Haiku), PDF.js, RAKE.js |
| **Architecture** | Tri-modal: Tier 1 (On-Device SLM) → Tier 2 (Cloud Bedrock) → Tier 3 (Deterministic RAKE) |
| **Build deadline** | 12 hours (10:00 PM Oct 3 → 10:00 AM Oct 4, 2026) |

---

## 2. Document References

When you need context beyond what's in this file, read the specific document. **Do NOT load all documents at once** — that wastes tokens and causes context rot.

| When you need... | Read this file | Why |
|-----------------|----------------|-----|
| What to build, feature scope, MVP boundaries | `prd.md` | Product requirements, feature tiers, what's in/out of scope |
| How the system works, architecture, data flow | `system-design.md` | Component diagram, tier detection logic, API contracts, IndexedDB schema |
| Why a feature exists, acceptance criteria | `user-stories.md` | User stories with survey-grounded acceptance criteria and test scripts |
| Colors, fonts, spacing, components, voice | `brand-guidelines.md` | Complete design system, Tailwind config, component specs, anti-patterns |
| Which document to consult for what | `index.md` | Master pointer to all FMDs |
| Raw research data and validation evidence | `research/` folder | Survey data, consulting firm evidence, competitive analysis |

**Rules for document access:**
- Load ONE document at a time, only when needed
- Never copy entire documents into your context window
- Extract only the specific section you need
- If a document contradicts this file, THIS FILE wins for agent behavior; the other document wins for its domain (e.g., `brand-guidelines.md` wins for color values)

---

## 3. Core Behavior Rules

### 3.1 You Are Building an MVP for a 12-Hour Hackathon

- **Ship working features, not perfect features.** A functional Tier 3 fallback is worth more than a polished Tier 1 that crashes.
- **2-3 strong features > 10 weak features.** The three core features are: Summarize, Quiz (adaptive), Chat. Everything else is secondary.
- **If a feature isn't in the MUST column of `prd.md`, don't build it** unless all MUST items are complete.
- **When in doubt, ask the human.** Don't spend 30 minutes guessing — ask.

### 3.2 Offline-First Is Non-Negotiable

Every feature you build MUST work in at least one offline tier. If you write code that requires internet to function and has no fallback, you have introduced a bug.

**Checklist before committing any feature:**
- [ ] Does this work with no internet? (Tier 3 minimum)
- [ ] Does this work with the SLM cached? (Tier 1)
- [ ] Does this gracefully upgrade when internet is available? (Tier 2)
- [ ] Is all user data stored in IndexedDB, not a remote server?

### 3.3 Zero Setup, Zero Accounts

- No login, no sign-up, no email collection, no OAuth — EVER
- No mandatory onboarding flow (optional 3-screen walkthrough, skippable)
- First meaningful action (upload PDF) must be available within 5 seconds of opening the app
- No analytics, no telemetry, no tracking cookies

### 3.4 Lightweight Is a Feature

Our target user has a 3-year-old Samsung Galaxy A13 (4GB RAM, 64GB storage). Code accordingly.

- App bundle (excluding AI models): < 500KB gzipped
- No heavy animation libraries (no Framer Motion, no GSAP, no Lottie)
- No 3D anything. No avatars. No social feeds. No complex dashboards.
- CSS transitions only — no JavaScript animation libraries
- Lazy-load everything that isn't on the first screen
- Unload the SLM from memory when not actively generating

---

## 4. Tech Stack Rules

### 4.1 Frontend

```
Framework:    React 18+ with Vite
Styling:      Tailwind CSS (config in brand-guidelines.md § 11)
Icons:        Heroicons (outline, 24px default)
Font:         Inter (variable, from Google Fonts CDN with local fallback)
State:        React Context + useReducer (no Redux, no Zustand — overkill for MVP)
Routing:      React Router v6 (3 routes max: Home, Document View, Settings)
PWA:          Vite PWA plugin (vite-plugin-pwa) with Workbox
```

**Do NOT install:**
- Next.js (SSR is pointless for an offline PWA)
- Redux, Zustand, Jotai, MobX (Context + useReducer is sufficient)
- Styled-components, Emotion (Tailwind handles everything)
- Framer Motion, GSAP, Lottie (violates lightweight rule)
- Any CSS framework besides Tailwind
- Any component library (Material UI, Chakra, Ant Design — too heavy)

### 4.2 Storage

```
Primary:      IndexedDB (via idb wrapper library)
Model cache:  Cache API (for ONNX + WASM model files)
App settings: localStorage (tier override, onboarding flag)
```

**IndexedDB stores:**
- `documents` — uploaded PDFs (title, rawText, chunks, embeddings, timestamp)
- `summaries` — generated summaries per document per tier
- `quizzes` — quiz history with questions, answers, scores
- `knowledgeState` — BKT mastery values per topic per document
- `chatHistory` — conversation history per document

Schema details are in `system-design.md` § IndexedDB Schema. Read that section when implementing storage.

### 4.3 AI / ML Pipeline

```
Embeddings:   MiniLM-L6-v2 (ONNX, 22MB, 384-dim output)
Runtime:       ONNX Runtime Web (WASM backend)
Vector search: HNSW index (hnswlib-wasm or custom)
SLM:           SmolLM2-1.7B-Instruct (Q4_K_M quantization, ~1GB)
SLM runtime:   llama.cpp compiled to WASM (via llama-cpp-wasm)
Cloud LLM:     Amazon Bedrock — Claude 3 Haiku (via Lambda + API Gateway)
Keyword extraction: RAKE.js (deterministic, no AI needed)
PDF parsing:   PDF.js (Mozilla, WASM-based)
```

### 4.4 Backend (Minimal)

```
API Gateway:  Amazon API Gateway (REST, single endpoint)
Compute:      AWS Lambda (Node.js 20.x runtime)
LLM:          Amazon Bedrock (Claude 3 Haiku — cheapest, fastest)
Auth:          None (no user accounts)
Database:     None (all data is client-side)
```

**Lambda function responsibilities:**
- Receive: `{ action: "summarize" | "quiz" | "chat", context: string, options: object }`
- Call Bedrock with appropriate system prompt
- Return structured JSON response
- Zero idle cost (serverless)

**The backend is a THIN PROXY to Bedrock. No business logic on the server.** All intelligence lives client-side.

---

## 5. Tier Detection Logic

This is the most critical piece of architecture. Implement it EXACTLY as specified.

```javascript
// tierDetection.js — runs on EVERY feature invocation

async function detectTier() {
  // Step 1: Check if user has manually locked a tier
  const override = localStorage.getItem('tierOverride');
  if (override && override !== 'auto') {
    return validateOverride(override);
  }

  // Step 2: Check Tier 1 — Is the SLM model cached?
  const modelCached = await caches.has('kaalaman-slm-model');
  
  // Step 3: Check Tier 2 — Is internet available?
  const online = navigator.onLine;
  let apiReachable = false;
  if (online) {
    try {
      const res = await fetch(API_HEALTH_ENDPOINT, { 
        method: 'HEAD', 
        signal: AbortSignal.timeout(3000) 
      });
      apiReachable = res.ok;
    } catch { 
      apiReachable = false; 
    }
  }

  // Step 4: Return best available tier
  if (apiReachable) return 'tier2';    // Cloud AI — best quality
  if (modelCached)  return 'tier1';    // On-Device AI — good quality
  return 'tier3';                       // Deterministic — always works
}
```

**Rules:**
- Tier detection runs on EVERY feature call (summarize, quiz, chat) — not just app launch
- If Tier 2 health check takes >3 seconds, fall to Tier 1 or 3 immediately
- NEVER show a loading spinner waiting for internet — fall through instantly
- Tier transitions show a toast notification, never a blocking modal
- Previously generated content remains accessible regardless of tier changes

---

## 6. Feature Implementation Order

Build in this exact sequence. Each step must be FULLY WORKING before moving to the next.

```
PHASE 1: FOUNDATION (Hours 1-3)
  ├── 1.1 Vite + React + Tailwind + PWA scaffold
  ├── 1.2 IndexedDB setup (all stores)
  ├── 1.3 PDF.js upload + text extraction
  ├── 1.4 Tier detection engine
  └── 1.5 Basic UI shell (home, document view, 3 tabs)

PHASE 2: TIER 3 — DETERMINISTIC (Hours 3-5)
  ├── 2.1 RAKE keyword extraction
  ├── 2.2 Template-based summarization (Tier 3)
  ├── 2.3 Fill-in-the-blank + True/False quiz generation (Tier 3)
  ├── 2.4 TF-IDF search for chat (Tier 3)
  └── 2.5 Tier badge UI (🟡 always visible)

PHASE 3: TIER 2 — CLOUD (Hours 5-7)
  ├── 3.1 Lambda + API Gateway deployment
  ├── 3.2 Bedrock summarization (structured JSON)
  ├── 3.3 Bedrock quiz generation (MCQ + short-answer, JSON)
  ├── 3.4 Bedrock RAG chat (with source citations)
  └── 3.5 Tier badge updates (🔵 when connected)

PHASE 4: ADAPTIVE LEARNING (Hours 7-9)
  ├── 4.1 Bayesian Knowledge Tracing (BKT) engine
  ├── 4.2 Mastery tracking per topic
  ├── 4.3 Weak-topic targeting in quiz generation
  ├── 4.4 Difficulty progression
  └── 4.5 Quiz results screen with mastery bars

PHASE 5: POLISH & DEMO PREP (Hours 9-12)
  ├── 5.1 Tier transition animations (toast notifications)
  ├── 5.2 Error handling for all edge cases
  ├── 5.3 PWA install prompt
  ├── 5.4 Lighthouse audit (PWA score >90)
  ├── 5.5 Demo rehearsal (7-step test script from user-stories.md)
  └── 5.6 Bug fixes from QA
```

**Critical rule:** If Phase 2 (Tier 3) is complete and working, you have a DEMOABLE product. Everything after that is enhancement. Never sacrifice a working Tier 3 to chase a broken Tier 1.

---

## 7. Prompt Engineering

All prompts are pre-engineered. Do NOT modify them during the build unless a specific output is broken.

### 7.1 System Prompt (Shared Across All Features)

```
You are KaalAman, a study assistant for Filipino college students. You ONLY answer based on the provided context from the student's uploaded notes. If the answer is not in the context, say: "I can't find this in your notes. Try uploading more materials."

Rules:
- NEVER use information from your training data
- NEVER hallucinate facts not in the context
- Use simple, clear language (like a helpful classmate)
- Keep responses concise (under 200 words for summaries, under 50 words for quiz explanations)
- If the student writes in Filipino/Taglish, respond in the same language
```

### 7.2 Summarization Prompt (Tier 2)

```
Given the following text from a student's notes, create a structured summary.

Return ONLY valid JSON:
{
  "overview": "3-5 sentence overview of the material",
  "key_concepts": [
    { "term": "...", "definition": "...", "importance": "high|medium|low" }
  ],
  "study_outline": [
    { "topic": "...", "subtopics": ["..."] }
  ],
  "common_mistakes": ["..."]
}

CONTEXT:
{chunks}
```

### 7.3 Quiz Generation Prompt (Tier 2)

```
Generate exactly 5 quiz questions from the following study material.
Student mastery context: {mastery_context}
Difficulty instruction: {difficulty_instruction}

Focus {weak_topic_count} questions on these weak topics: {weak_topics}

Return ONLY valid JSON:
{
  "questions": [
    {
      "id": 1,
      "type": "mcq",
      "topic": "topic name",
      "question": "...",
      "options": ["A) ...", "B) ...", "C) ...", "D) ..."],
      "correct": "B",
      "explanation": "Why B is correct (under 30 words)",
      "distractor_explanations": {
        "A": "Why A is wrong (under 15 words)",
        "C": "Why C is wrong (under 15 words)",
        "D": "Why D is wrong (under 15 words)"
      }
    }
  ]
}

CONTEXT:
{chunks}
```

### 7.4 Chat Prompt (Tier 2)

```
Answer the student's question using ONLY the provided context passages.

If the answer is not in the context, respond with:
{ "answer": "I can't find this in your notes.", "sources": [], "confidence": 0, "follow_up_suggestion": null }

Return ONLY valid JSON:
{
  "answer": "Your answer here (under 150 words)",
  "sources": ["exact quote from context"],
  "confidence": 0.0-1.0,
  "follow_up_suggestion": "A natural follow-up question"
}

CONTEXT PASSAGES:
{retrieved_chunks}

CONVERSATION HISTORY:
{last_n_turns}

STUDENT'S QUESTION:
{question}
```

### 7.5 Difficulty Instructions (BKT-Driven)

```javascript
function getDifficultyInstruction(avgMastery) {
  if (avgMastery < 0.3) {
    return "Focus on foundational concepts. Basic recall and definition questions. Use simple language.";
  } else if (avgMastery < 0.7) {
    return "Mix foundational and application questions. 2 recall + 3 application. Include 'why' and 'how' questions.";
  } else {
    return "Focus on application and analysis. Scenario-based questions. Ask students to compare, contrast, or predict.";
  }
}
```

---

## 8. BKT (Bayesian Knowledge Tracing) Implementation

```javascript
// bkt.js — Bayesian Knowledge Tracing engine

const BKT_DEFAULTS = {
  pInit: 0.3,    // Prior probability of knowing the skill
  pLearn: 0.2,   // Probability of learning after an opportunity
  pSlip: 0.1,    // Probability of incorrect answer despite knowing
  pGuess: 0.25,  // Probability of correct answer despite not knowing
};

function updateMastery(currentMastery, isCorrect, params = BKT_DEFAULTS) {
  const { pSlip, pGuess, pLearn } = params;
  const pKnown = currentMastery;

  // Posterior update
  let posterior;
  if (isCorrect) {
    const pCorrectGivenKnown = 1 - pSlip;
    const pCorrectGivenUnknown = pGuess;
    const pCorrect = pKnown * pCorrectGivenKnown + (1 - pKnown) * pCorrectGivenUnknown;
    posterior = (pKnown * pCorrectGivenKnown) / pCorrect;
  } else {
    const pIncorrectGivenKnown = pSlip;
    const pIncorrectGivenUnknown = 1 - pGuess;
    const pIncorrect = pKnown * pIncorrectGivenKnown + (1 - pKnown) * pIncorrectGivenUnknown;
    posterior = (pKnown * pIncorrectGivenKnown) / pIncorrect;
  }

  // Learning update
  const updatedMastery = posterior + (1 - posterior) * pLearn;

  // Clamp to [0.01, 0.99] — never absolute certainty
  return Math.max(0.01, Math.min(0.99, updatedMastery));
}

function getWeakTopics(knowledgeState, threshold = 0.6) {
  return Object.entries(knowledgeState)
    .filter(([_, mastery]) => mastery < threshold)
    .sort((a, b) => a[1] - b[1])
    .map(([topic, mastery]) => ({ topic, mastery }));
}
```

**Rules:**
- BKT runs after EVERY quiz answer (not just at the end)
- Knowledge state persists in IndexedDB across sessions
- Mastery values are per-topic, per-document
- Slip protection: a single wrong answer on a mastered topic (>0.8) should not crash mastery below 0.6

---

## 9. File & Folder Structure

```
kaalaman/
├── public/
│   ├── icons/                    # PWA icons (192, 512, maskable)
│   ├── manifest.json             # PWA manifest (from brand-guidelines.md § 12)
│   └── sw.js                     # Service worker (generated by vite-plugin-pwa)
├── src/
│   ├── main.jsx                  # App entry point
│   ├── App.jsx                   # Router + Context providers
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Header.jsx        # App header with tier badge
│   │   │   ├── BottomNav.jsx     # Tab bar (Summary, Quiz, Chat)
│   │   │   └── TierBadge.jsx     # 🟢🔵🟡 indicator
│   │   ├── upload/
│   │   │   ├── UploadButton.jsx  # PDF upload trigger
│   │   │   └── ProcessingBar.jsx # Extraction + embedding progress
│   │   ├── summary/
│   │   │   └── SummaryView.jsx   # Renders summary (all tiers)
│   │   ├── quiz/
│   │   │   ├── QuizView.jsx      # Quiz flow controller
│   │   │   ├── QuestionCard.jsx  # Single question display
│   │   │   └── ResultsView.jsx   # Score + mastery bars
│   │   ├── chat/
│   │   │   ├── ChatView.jsx      # Chat interface
│   │   │   ├── ChatBubble.jsx    # User/assistant message
│   │   │   └── SourceCard.jsx    # Citation display
│   │   └── common/
│   │       ├── Toast.jsx         # Tier transition notifications
│   │       ├── EmptyState.jsx    # "Upload your first PDF"
│   │       └── SkeletonLoader.jsx # Loading placeholder
│   ├── pages/
│   │   ├── HomePage.jsx          # Document list
│   │   ├── DocumentPage.jsx      # Tabs: Summary | Quiz | Chat
│   │   └── SettingsPage.jsx      # Storage, tier override
│   ├── services/
│   │   ├── tierDetection.js      # Tier detection engine (§ 5)
│   │   ├── pdfExtractor.js       # PDF.js text extraction
│   │   ├── embeddings.js         # MiniLM ONNX embedding generation
│   │   ├── vectorSearch.js       # HNSW index build + query
│   │   ├── summarizer.js         # Tri-tier summarization
│   │   ├── quizGenerator.js      # Tri-tier quiz generation
│   │   ├── chatEngine.js         # Tri-tier RAG chat
│   │   ├── bkt.js                # Bayesian Knowledge Tracing (§ 8)
│   │   ├── rake.js               # RAKE keyword extraction (Tier 3)
│   │   └── bedrockClient.js      # API Gateway → Lambda → Bedrock
│   ├── stores/
│   │   ├── db.js                 # IndexedDB setup (idb wrapper)
│   │   ├── documentStore.js      # CRUD for documents
│   │   ├── quizStore.js          # CRUD for quizzes + knowledge state
│   │   └── settingsStore.js      # App settings (localStorage)
│   ├── context/
│   │   ├── TierContext.jsx       # Current tier state + transitions
│   │   └── DocumentContext.jsx   # Active document state
│   ├── hooks/
│   │   ├── useTier.js            # Hook for tier detection
│   │   ├── useDocument.js        # Hook for active document
│   │   └── useBKT.js             # Hook for knowledge tracing
│   ├── utils/
│   │   ├── chunker.js            # Text chunking (sentence-aware)
│   │   ├── tfidf.js              # TF-IDF for Tier 3 search
│   │   └── promptBuilder.js      # Builds prompts from templates (§ 7)
│   └── styles/
│       └── index.css             # Tailwind directives + custom utilities
├── docs/                          # Foundational Matrix Documents
│   ├── index.md                  # Master pointer to all FMDs
│   ├── agents.md                 # THIS FILE
│   ├── prd.md                    # Product Requirements Document v3.0
│   ├── system-design.md          # System Design Document v3.0
│   ├── user-stories.md           # User Stories v3.0
│   ├── brand-guidelines.md       # Brand Guidelines v3.0
│   └── research/                 # Validation evidence
│       ├── survey-data.csv       # Raw survey (n=40)
│       └── consulting-evidence.md # Big 4 + MBB findings
├── infra/                         # AWS infrastructure
│   ├── lambda/
│   │   └── handler.js            # Lambda function (Bedrock proxy)
│   └── template.yaml             # SAM/CloudFormation template
├── tailwind.config.js            # From brand-guidelines.md § 11
├── vite.config.js                # Vite + PWA plugin config
├── package.json
└── README.md
```

**Rules:**
- One component per file. No god-components.
- Services handle business logic. Components handle UI only.
- Every service must export functions that work for ALL THREE TIERS.
- No circular imports. Services never import from components.

---

## 10. Code Style & Conventions

### 10.1 General

```javascript
// Naming
const myVariable = 'camelCase for variables and functions';
const MyComponent = () => {};  // PascalCase for React components
const API_ENDPOINT = '...';    // UPPER_SNAKE for constants
const db-store-name = '...';   // kebab-case for CSS classes (Tailwind handles this)

// Functions
// - Use arrow functions for components and callbacks
// - Use async/await, never raw Promises with .then()
// - Every async function must have try/catch with meaningful error handling
// - Never swallow errors silently — at minimum, console.error + user-facing toast

// Imports
// - Group: React → third-party → local services → local components → styles
// - Use named exports, not default exports (except for pages)
```

### 10.2 Error Handling Pattern

```javascript
// EVERY service function follows this pattern:
async function summarize(documentId) {
  try {
    const tier = await detectTier();
    
    switch (tier) {
      case 'tier2': return await summarizeTier2(documentId);
      case 'tier1': return await summarizeTier1(documentId);
      case 'tier3': return await summarizeTier3(documentId);
      default:      return await summarizeTier3(documentId); // Always fall to Tier 3
    }
  } catch (error) {
    console.error('[Summarizer] Failed:', error);
    
    // If Tier 2 or 1 failed, fall to Tier 3 — NEVER show an error to the user
    // if a lower tier can handle it
    try {
      return await summarizeTier3(documentId);
    } catch (fallbackError) {
      // Only now show an error — all tiers failed
      throw new Error('Unable to generate summary. Please try again.');
    }
  }
}
```

### 10.3 Component Pattern

```jsx
// Every component follows this structure:
import { useState } from 'react';
import { useTier } from '../hooks/useTier';

export function QuestionCard({ question, onAnswer }) {
  const { currentTier } = useTier();
  const [selected, setSelected] = useState(null);

  // Event handlers
  const handleSelect = (option) => setSelected(option);
  const handleSubmit = () => onAnswer(question.id, selected);

  // Render
  return (
    <div className="bg-surface rounded-card p-4 shadow-card">
      {/* Component JSX — uses Tailwind classes from brand-guidelines.md */}
    </div>
  );
}
```

---

## 11. Git Workflow

### 11.1 Branch Strategy

```
main              ← Production-ready (deploy from here)
├── dev           ← Integration branch (merge features here first)
├── feat/upload   ← PDF upload + extraction
├── feat/tier3    ← Tier 3 deterministic features
├── feat/tier2    ← Tier 2 cloud features
├── feat/bkt      ← Adaptive learning engine
└── fix/*         ← Bug fixes
```

### 11.2 Commit Messages

```
feat: add PDF text extraction with PDF.js
feat: implement RAKE summarization (Tier 3)
feat: add Bedrock quiz generation (Tier 2)
fix: tier detection falls to Tier 3 when API times out
refactor: extract chunker into separate utility
style: apply brand colors to quiz result cards
docs: update system-design.md with IndexedDB schema
```

### 11.3 Shift Handoff Protocol

When one developer finishes their shift and another takes over:

1. **Outgoing dev:** Commit all work, push to `dev`, write a brief comment in the last commit: "HANDOFF: Tier 3 summarization complete. Quiz generation next. See Phase 2 in agents.md § 6."
2. **Incoming dev:** Pull `dev`, read the last 3 commit messages, check agents.md § 6 for current phase, continue from there.
3. **Never start a new feature without checking what phase the project is in.**

---

## 12. Testing Checklist (Pre-Submission)

Before submitting, run through this checklist. Every item must pass.

```
FUNCTIONALITY
[ ] PDF upload works (5-page and 10-page test)
[ ] Summary generates on at least 2 tiers
[ ] Quiz generates 5 questions on at least 2 tiers
[ ] Quiz shows correct/incorrect with explanations
[ ] Mastery bars appear after quiz completion
[ ] Second quiz targets weak topics (≥3 of 5 questions)
[ ] Chat answers questions from the document
[ ] Chat refuses questions NOT in the document
[ ] All data persists after closing and reopening the app

OFFLINE
[ ] Turn off Wi-Fi → app still loads
[ ] Turn off Wi-Fi → summary generates (Tier 3)
[ ] Turn off Wi-Fi → quiz generates (Tier 3)
[ ] Turn off Wi-Fi → chat retrieves passages (Tier 3)
[ ] Tier badge changes when connectivity changes
[ ] No feature shows "connect to internet" error

PWA
[ ] App is installable on Android home screen
[ ] Lighthouse PWA score > 90
[ ] Service worker caches app shell
[ ] manifest.json is valid

PERFORMANCE
[ ] First contentful paint < 2 seconds
[ ] PDF extraction < 15 seconds (10 pages)
[ ] Tier 3 summary < 1 second
[ ] Tier 2 summary < 3 seconds
[ ] App bundle < 500KB gzipped (excluding models)

UI/UX
[ ] All text ≥ 14px
[ ] All touch targets ≥ 44px
[ ] Colors match brand-guidelines.md
[ ] No 3D avatars, no social feeds, no complex dashboards
[ ] Empty states show helpful messages
[ ] Error messages are human-readable (not stack traces)
```

---

## 13. What NOT to Do

These are the most common ways hackathon teams waste time. Avoid all of them.

| Anti-Pattern | Why It's Deadly | What to Do Instead |
|-------------|----------------|-------------------|
| **Building Tier 1 first** | SLM WASM setup is complex and fragile; if it fails, you have nothing to demo | Build Tier 3 first (always works), then Tier 2 (cloud), then Tier 1 (SLM) |
| **Perfecting UI before features work** | Pretty buttons that do nothing score 0 on "MVP & Technical Implementation" (30%) | Get features working with basic UI, then polish in Phase 5 |
| **Adding features not in the PRD** | Scope creep kills hackathon teams | If it's not in `prd.md` MUST column, it doesn't exist tonight |
| **Debugging for >20 minutes** | You have 12 hours; 20 minutes is 2.8% of your total time | If stuck >15 min, revert to last working state and try a different approach |
| **Not committing frequently** | One bad change can destroy hours of work | Commit after every working feature, even if it's ugly |
| **Loading all FMDs into context** | Wastes tokens, causes context rot, agent gets confused | Load ONE document at a time, extract only the section you need |
| **Building dark mode** | Doubles CSS work for zero judging points | Light mode only. Dark mode is post-hackathon. |
| **Custom authentication** | No users = no auth needed; this wastes 1-2 hours minimum | Zero accounts. localStorage + IndexedDB. Done. |
| **Over-engineering the backend** | The backend is a thin Bedrock proxy. Nothing more. | One Lambda, one API Gateway endpoint, one Bedrock call. Ship it. |
| **Ignoring Tier 3** | If cloud goes down during demo, you're dead | Tier 3 is your safety net. It must ALWAYS work. |

---

## 14. Kiro-Specific Configuration

Since Kiro is the required IDE for this hackathon, configure it as follows:

### 14.1 `.kiro/settings.json`

```json
{
  "agent": {
    "instructionsFile": "docs/agents.md",
    "contextFiles": ["docs/index.md"],
    "maxContextTokens": 8000,
    "autoReadDocuments": false
  }
}
```

### 14.2 Kiro Spec-Driven Development

Kiro supports spec-driven development. Map our FMDs to Kiro specs:

| Kiro Concept | Our FMD Equivalent |
|-------------|-------------------|
| Requirements spec | `prd.md` |
| Design spec | `system-design.md` |
| Task list | Phase list in this file (§ 6) |
| Acceptance criteria | `user-stories.md` acceptance criteria |

When Kiro asks for a spec, point it to the relevant FMD. Do NOT rewrite specs — our documents are already in the right format.

### 14.3 Parallel Agent Workflows

Kiro supports parallel agent workflows. Use them for independent tasks:

```
Agent 1: Build Tier 3 summarization (RAKE + templates)
Agent 2: Build Tier 3 quiz generation (fill-in-blank + T/F)
Agent 3: Set up IndexedDB stores + PDF extraction

These are independent — they can run in parallel.

DO NOT parallelize:
- Tier 2 features (they depend on Tier 3 being done first for fallback)
- BKT engine (depends on quiz generation being done)
- UI polish (depends on all features being functional)
```

---

## 15. Context Rot Prevention

The hackathon strategies talk warned about "context poisoning" — when too many documents or conflicting versions confuse the agent. Here's how we prevent it:

### Rules:
1. **This file (agents.md) is version 3.0. There is no v1 or v2.** If you encounter references to earlier versions, ignore them.
2. **All FMDs are v3.0.** They are the ONLY authoritative source. No earlier versions exist in this repo.
3. **If you need to pivot during the build**, update the relevant FMD immediately. Don't create a new file — edit the existing one. Git history preserves the old version.
4. **Never have two files that describe the same thing.** One source of truth per topic.
5. **If a teammate verbally tells you something that contradicts an FMD**, update the FMD first, then implement. The document is the contract.

### Document Authority Hierarchy:
```
1. agents.md        → Agent behavior (THIS FILE — highest authority for how to code)
2. prd.md           → What to build (highest authority for scope)
3. system-design.md → How it works (highest authority for architecture)
4. user-stories.md  → Why it exists (highest authority for acceptance criteria)
5. brand-guidelines.md → How it looks (highest authority for design)
6. index.md         → Where to find things (pointer only)
```

If two documents conflict, the one higher in this list wins for its domain.

---

## 16. Emergency Protocols

### "It's 4 AM and nothing works"

1. `git stash` your current changes
2. `git checkout dev` — go back to last known working state
3. Identify which phase was last completed successfully (§ 6)
4. Restart from the next phase with a simpler approach
5. Remember: a working Tier 3 demo beats a broken Tier 1 demo every time

### "The SLM WASM won't load"

1. Don't panic. Tier 1 is a SHOULD, not a MUST.
2. Ensure Tier 2 (cloud) and Tier 3 (deterministic) work perfectly
3. In the demo, show Tier 2 → turn off Wi-Fi → show Tier 3. That's still impressive.
4. Mention Tier 1 as "ready for deployment" in the pitch — judges care about the architecture, not just the demo

### "Bedrock API is rate-limited / down"

1. Tier 2 is not available. That's fine.
2. Ensure Tier 3 works perfectly
3. Show the Lambda code and API Gateway config to prove Tier 2 exists
4. In the pitch: "Our cloud tier uses Bedrock via Lambda. Here's the code. Tonight we're demonstrating our offline resilience."

### "We're running out of time"

1. Stop building new features immediately
2. Check the testing checklist (§ 12) — what passes? What fails?
3. Fix only the failing items that affect the demo flow
4. Rehearse the 7-step demo script from `user-stories.md`
5. A polished demo of 2 features beats a buggy demo of 5 features
