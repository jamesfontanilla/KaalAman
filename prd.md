
# Product Requirements Document (PRD) v3.0
# KaalAman — Offline-First Edge-AI Study Companion

## Document Info
| Field | Value |
|-------|-------|
| Version | 3.0 (Post-Merge + Post-Roast) |
| Last Updated | October 3, 2026 |
| Status | FINAL — Locked for Build |
| Team | 4-person team, Build Over Nights Hackathon |

---

## 1. Product Overview

**KaalAman** is an offline-first, edge-AI study companion that transforms uploaded PDFs and notes into structured summaries, adaptive quizzes, and a RAG-powered chat — all functioning without internet through a three-tier graceful degradation architecture.

**One-line pitch:** "Upload your notes, study anywhere — even offline."

**Tagline:** "Your notes. Your device. Your pace."

---

## 2. Problem Statement

Filipino college students face compounding barriers to effective studying:

### 2.1 The Crisis (Macro Evidence)
- **91% learning poverty rate** — children unable to read proficiently by late primary age (World Bank)
- **PISA 2025 scores lag OECD by ~100 points** across reading, math, and science
- **Only 48.8% of households have internet access** (PSA 2023)
- **1.5 million youth out of school** (DepEd)
- **ChatGPT Plus at $20/month = 9.6%** of a poverty-line family's monthly budget

### 2.2 The Friction (Micro Evidence — Our Survey, n=40)
- **67.5%** rely exclusively on free-tier apps (Q3)
- **85%** prefer an offline-capable, free tool over a paid cloud tool (Q12)
- **55%** willing to pay ≤₱100/month or nothing (Q14)
- **Friction scores average 3.1/5** for paywalls, crashes, and setup time (Q6)
- **Median estimate: 6-8 out of 10 peers** locked out of premium AI tools (Q9)
- **Top workarounds:** sharing accounts, multiple free trials, reverting to Google Docs + notebooks (Q7)

### 2.3 Evidence Caveats (We Own These)
- n=40, single community, self-selected convenience sample
- Q12 confounds "offline" with "free" and "simple" — cannot isolate which attribute drives preference
- Friction scores are medium (3/5 = "sometimes"), not crisis-level
- 50% would drop digital tools entirely if budget cut further (fails recession test for half)
- No learning outcome data — only frustration and preference data
- Triangulated with World Bank, PISA, BCG, Kenya deployment data to compensate

### 2.4 Problem Statement (One Sentence)
> Filipino college students who can't afford premium AI tools and lack reliable internet have no way to turn their course notes into effective study materials on their own devices.

---

## 3. Target User — Ideal Customer Profile (ICP)

### 3.1 Primary Persona: "Maria"
| Attribute | Detail |
|-----------|--------|
| Age | 18-24 |
| Role | Filipino college student (self-directed learner) |
| Location | Metro Manila or provincial city, Philippines |
| Device | Budget Android phone (2-4GB RAM) or 3-5 year old laptop |
| Internet | Spotty prepaid mobile data, campus Wi-Fi when available |
| Budget | ≤₱100/month for study tools (most prefer free) |
| Current tools | Free ChatGPT, Google Docs, Quizlet free tier, physical notebooks |
| Pain | Hits paywalls mid-study, apps crash on old device, loses momentum when offline |
| Goal | Pass exams, understand course material, study efficiently on her own schedule |

### 3.2 Secondary Persona: "Prof. Santos"
| Attribute | Detail |
|-----------|--------|
| Role | College instructor at a provincial university |
| Pain | Students come unprepared, no budget for institutional AI tools |
| Goal | Recommend a free tool students can actually use |
| KaalAman value | Institutional licensing (future), student analytics dashboard (future) |

---

## 4. Solution — Three-Tier Graceful Degradation

### 4.1 Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                    KaalAman PWA                       │
│                                                       │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐            │
│  │ Upload  │──▶│ Process │──▶│ Study   │            │
│  │ (PDF.js)│   │ (Chunk) │   │ Tools   │            │
│  └─────────┘   └────┬────┘   └────┬────┘            │
│                      │             │                  │
│              ┌───────▼─────────────▼───────┐         │
│              │    TIER DETECTION ENGINE     │         │
│              │  (RAM + WASM + Connectivity) │         │
│              └───┬─────────┬─────────┬─────┘         │
│                  │         │         │                │
│           ┌──────▼──┐ ┌───▼────┐ ┌──▼───────┐       │
│           │ TIER 1  │ │ TIER 2 │ │ TIER 3   │       │
│           │ Edge-AI │ │ Cloud  │ │ Fallback │       │
│           │ (WASM)  │ │(Bedrock│ │ (RAKE+   │       │
│           │         │ │+Lambda)│ │ Templates│       │
│           └─────────┘ └────────┘ └──────────┘       │
│                                                       │
│  ┌─────────────────────────────────────────────┐     │
│  │         IndexedDB (Dexie.js)                 │     │
│  │  Documents | Summaries | Quizzes | KT State  │     │
│  └─────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────┘
```

### 4.2 Tier Specifications

| Aspect | Tier 1: Edge-AI | Tier 2: Cloud-AI | Tier 3: Deterministic |
|--------|----------------|-------------------|----------------------|
| **Engine** | Phi-3-mini Q4_K_M via llama.cpp-WASM | Claude 3 Haiku via Amazon Bedrock | RAKE.js + templates |
| **Requires** | ≥4GB RAM, WASM support, model cached | Internet connection | Nothing (always works) |
| **Latency** | 3-8 sec (on-device inference) | 1-3 sec (API call) | <1 sec (deterministic) |
| **Quality** | Good (80-85% of cloud quality) | Best (full LLM capability) | Functional (60-70% quality) |
| **Cost** | Free (on-device) | ~$0.001/call (Bedrock Haiku) | Free (no compute) |
| **Model size** | ~2.2GB GGUF file (one-time download) | N/A (server-side) | ~50KB (RAKE library) |
| **RAM usage** | ~1.2-1.5GB during inference | Minimal (API call only) | Minimal |
| **Offline?** | ✅ Yes (after model cached) | ❌ No | ✅ Yes |

### 4.3 Tier Detection Logic

```
function detectTier():
  IF model cached in IndexedDB AND deviceMemory ≥ 4GB AND WASM supported:
    → TRY Tier 1 (run micro-benchmark, confirm inference works)
    → IF benchmark fails: fall through to Tier 2
  
  IF navigator.onLine AND fetch("health-check-endpoint") succeeds:
    → USE Tier 2 (Bedrock via Lambda)
  
  ELSE:
    → USE Tier 3 (RAKE + templates)
  
  // Re-evaluate on every feature invocation (not just app launch)
  // Show tier indicator badge: 🟢 Tier 1 | 🔵 Tier 2 | 🟡 Tier 3
```

---

## 5. Feature Requirements

### 5.1 Feature 1: Smart Summarization

| Requirement | Detail |
|-------------|--------|
| **Input** | Uploaded PDF or pasted text |
| **Output** | Structured summary with: overview paragraphs, key concepts list, study outline |
| **Tier 1** | SLM generates markdown-formatted summary (system + user prompt) |
| **Tier 2** | Bedrock generates JSON summary with additional fields: importance per concept, common mistakes |
| **Tier 3** | RAKE extracts top 10 keywords + top 5 keyword-dense sentences + topic outline from section headers |
| **Storage** | Summary saved to IndexedDB, linked to document ID |
| **Acceptance criteria** | Summary covers all major topics from input; key concepts are actually key (not random details); output renders correctly in UI |

### 5.2 Feature 2: Adaptive Quiz Generation

| Requirement | Detail |
|-------------|--------|
| **Input** | Document text + student's knowledge state (from Bayesian KT) |
| **Output** | 5 questions: mix of MCQ and short-answer, difficulty adapted to mastery |
| **Tier 1** | SLM generates text-formatted quiz; difficulty instruction injected from BKT |
| **Tier 2** | Bedrock generates JSON quiz with distractor explanations and acceptable answer alternatives |
| **Tier 3** | Fill-in-the-blank (keyword removal from sentences) + True/False (real vs. keyword-swapped sentences) |
| **Adaptive logic** | Bayesian Knowledge Tracing (BKT) tracks per-topic mastery; weak topics get more questions; difficulty scales with average mastery |
| **Storage** | Quiz results + updated knowledge state saved to IndexedDB |
| **Acceptance criteria** | Questions are answerable from the source material; wrong answers are plausible; difficulty adapts after 3+ quiz attempts on same material |

#### Bayesian Knowledge Tracing Parameters
```
pInit  = 0.3   // Prior probability student knows concept
pLearn = 0.2   // Probability of learning per opportunity
pSlip  = 0.1   // Probability of wrong answer despite knowing
pGuess = 0.25  // Probability of right answer despite not knowing (1/4 for MCQ)
```

### 5.3 Feature 3: RAG-Powered Chat

| Requirement | Detail |
|-------------|--------|
| **Input** | Student's question + retrieved context chunks from their uploaded documents |
| **Output** | Grounded answer citing only the student's own notes |
| **Embedding model** | all-MiniLM-L6-v2 (ONNX quantized, runs in browser) |
| **Vector store** | HNSW index (hnswlib-wasm), stored in IndexedDB |
| **Tier 1** | Top 3 chunks + last 2 conversation turns → SLM generates answer |
| **Tier 2** | Top 5 chunks + last 5 turns → Bedrock generates JSON answer with source quotes + confidence + follow-up suggestion |
| **Tier 3** | TF-IDF keyword search → returns top 3 matching passages with document name + page number (no generation) |
| **Hallucination guard** | System prompt enforces: "ONLY answer from provided context. If not found, say so." |
| **Storage** | Chat history saved to IndexedDB per document |
| **Acceptance criteria** | Answers are grounded in uploaded notes; refuses to answer questions not in context; follow-up questions work |

### 5.4 Feature 0: Document Upload & Processing

| Requirement | Detail |
|-------------|--------|
| **Input** | PDF file (via file picker or camera/photo) |
| **Processing pipeline** | PDF.js extracts text → sentence-aware chunking (400 tokens for Tier 1, 800 for Tier 2) → MiniLM generates embeddings → HNSW index built → all stored in IndexedDB |
| **Acceptance criteria** | PDF text extracted accurately; chunks maintain sentence boundaries; embeddings generated without crashing on budget device |

---

## 6. Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| **First meaningful interaction** | <30 seconds from PDF upload to first summary |
| **Offline capability** | All Tier 1 and Tier 3 features work with zero internet |
| **PWA installable** | Passes Lighthouse PWA audit; installable on Android home screen |
| **Storage efficiency** | <3GB total (model + app + cached data) |
| **Device support** | Android 8+ with Chrome 90+; budget devices with 2GB+ RAM (Tier 3), 4GB+ RAM (Tier 1) |
| **Accessibility** | Minimum AA contrast ratios; readable at default font size on 5" screen |
| **Privacy** | All student data stays on-device (Tier 1 & 3); Tier 2 sends only text chunks to Bedrock (no PII) |

---

## 7. Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend framework | React 18 + Vite | Fast builds, large ecosystem, team familiarity |
| Styling | Tailwind CSS 3 | Utility-first, rapid prototyping, small bundle |
| PWA | Workbox (service worker) | Reliable offline caching, precaching strategies |
| Local database | Dexie.js (IndexedDB wrapper) | Simple API, reactive queries, good for structured data |
| PDF extraction | PDF.js (pdfjs-dist) | Mozilla's battle-tested PDF renderer/extractor |
| On-device LLM | llama.cpp → WASM (via @anthropic/llama-cpp-wasm or similar) | Best WASM LLM runtime, supports GGUF quantized models |
| On-device embeddings | ONNX Runtime Web + all-MiniLM-L6-v2 | Fast, quantized, runs in browser |
| Vector search | hnswlib-wasm | HNSW algorithm, fast approximate nearest neighbor |
| Keyword extraction | RAKE.js | Lightweight, no dependencies, works offline |
| Cloud LLM | Amazon Bedrock (Claude 3 Haiku) | Low cost, fast, good instruction following |
| Cloud compute | AWS Lambda (Node.js 20) | Serverless, zero idle cost, scales to zero |
| API layer | Amazon API Gateway (HTTP API) | Low latency, cheap, integrates with Lambda |
| Dev environment | Kiro | Required by hackathon |
| Research/validation | Amazon Quick | Required by hackathon |

---

## 8. MVP Scope (12-Hour Build Window)

### 8.1 MUST HAVE (Demo-Ready)
- [ ] PDF upload → text extraction → chunking → embedding → IndexedDB storage
- [ ] Tier detection engine (auto-selects tier on each feature invocation)
- [ ] Tier indicator badge visible on all screens (🟢🔵🟡)
- [ ] Summarization working on at least 2 tiers (Tier 2 + Tier 3 minimum)
- [ ] Quiz generation working on at least 2 tiers with BKT adaptation
- [ ] RAG chat working on at least 2 tiers with hallucination refusal
- [ ] PWA installable (manifest.json + service worker)
- [ ] Mobile-responsive UI

### 8.2 SHOULD HAVE (If Time Permits)
- [ ] Tier 1 (on-device SLM) fully working
- [ ] All 3 tiers working for all 3 features (9/9 matrix)
- [ ] Smooth tier transition animations
- [ ] Model download progress indicator
- [ ] Multiple document management

### 8.3 WON'T HAVE (Post-Hackathon)
- ❌ User accounts / authentication
- ❌ Cloud sync across devices
- ❌ Institutional dashboard
- ❌ Spaced repetition scheduling
- ❌ Social features / pack sharing
- ❌ OCR from camera photos
- ❌ Multi-language support beyond English + Taglish

---

## 9. Revenue Model (Post-Hackathon Vision)

| Tier | Price | What You Get |
|------|-------|-------------|
| Free | ₱0 | Tier 3 (deterministic) + Tier 1 (if device supports it) — unlimited, forever |
| Pro | ₱49/month (~$0.85) | Tier 2 (Bedrock cloud AI) — higher quality summaries, richer quizzes, full RAG chat |
| Institutional | Custom pricing | Bulk licensing for universities, analytics dashboard, LMS integration |

**Unit economics:** Bedrock Haiku costs ~$0.001/call. At 50 calls/student/month = $0.05/student. ₱49 revenue (~$0.85) = 94% gross margin.

---

## 10. Success Metrics

| Metric | Target (Hackathon) | Target (6-Month Post-Launch) |
|--------|--------------------|-----------------------------|
| Features working | ≥6/9 tier-feature combinations | 9/9 |
| Demo completeness | Full loop: upload → summarize → quiz → chat | — |
| Pitch score | Top 3 in Education track | — |
| User adoption | — | 1,000 active students |
| Learning outcomes | — | Measurable improvement in self-reported study confidence |
| Tier 3 usage | — | >40% of sessions use Tier 3 (validates offline need) |

---

## 11. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| llama.cpp-WASM crashes on budget devices | High | High | Tier 2 + Tier 3 fallback; test on actual hardware pre-build |
| 2.2GB model download too slow | High | Medium | Progressive download; Tier 2/3 work immediately while model downloads |
| Bedrock API latency spikes | Low | Medium | Client-side timeout → fall to Tier 3; cache responses |
| RAKE produces garbage keywords | Medium | Medium | Test on real academic text pre-build; have TF-IDF backup |
| JSON parsing fails from LLM output | Medium | High | Regex fallback parser; retry with stricter prompt; Tier 3 never fails |
| 12 hours isn't enough | High | Critical | Pre-work maximized; MVP scoped to 6/9 minimum; cut Tier 1 if behind |
| Judges question n=40 sample | High | Medium | Own the limitation; triangulate with World Bank + BCG + Kenya data |

---

## 12. Hackathon Integration Requirements

### 12.1 Kiro Integration
- All development done in Kiro IDE
- `agents.md` defines Kiro behavior and coding standards
- Kiro reads foundational documents via `index.md` pointer
- Commit history shows Kiro-assisted development

### 12.2 Amazon Quick Integration
- Research reports generated via Quick Research (already done)
- Quick Spaces used as knowledge hub for validation data
- Quick Chat used for iterative analysis during build
- Mention in pitch: "We used Amazon Quick to validate our problem with Big 4 and MBB consulting data"

### 12.3 AWS Services
- Amazon Bedrock (Claude 3 Haiku) — Tier 2 LLM inference
- AWS Lambda — serverless compute for Tier 2
- Amazon API Gateway — HTTP API for Tier 2 endpoints
- (Optional) S3 — model file hosting for Tier 1 download
