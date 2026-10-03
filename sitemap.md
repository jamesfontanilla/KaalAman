
# Sitemap v3.0
# KaalAman — Page Hierarchy, Navigation Flow & Component Map

## Document Info
| Field | Value |
|-------|-------|
| Version | 3.0 (Post-Merge + Post-Roast) |
| Last Updated | October 3, 2026 |
| Status | FINAL — Locked for Build |
| Related | PRD v3.0, System Design v3.0, User Stories v3.0, Brand Guidelines v3.0 |
| Purpose | Shows every page, what it contains, how pages connect, and how the user moves through the app |

---

## 1. Page Hierarchy (Bird's-Eye View)

```
KaalAman (PWA)
│
├── 🏠 HOME PAGE (/)
│   ├── Document List
│   ├── Upload Button (FAB)
│   ├── Empty State (first visit)
│   └── → Settings (gear icon)
│
├── 📄 DOCUMENT PAGE (/doc/:id)
│   ├── Document Header (title, tier badge, back button)
│   ├── Tab Bar ─────────────────────────────────┐
│   │   ├── 📝 Summary Tab (/doc/:id/summary)    │
│   │   ├── ❓ Quiz Tab (/doc/:id/quiz)           │ ← Bottom nav
│   │   └── 💬 Chat Tab (/doc/:id/chat)           │
│   │                                              │
│   │   SUMMARY TAB                                │
│   │   ├── Tier Badge (🟢🔵🟡)                   │
│   │   ├── Overview Section                       │
│   │   ├── Key Concepts Section                   │
│   │   ├── Study Outline Section                  │
│   │   ├── Common Mistakes (Tier 2 only)          │
│   │   └── Regenerate Button                      │
│   │                                              │
│   │   QUIZ TAB                                   │
│   │   ├── Quiz Start Screen (or auto-start)      │
│   │   ├── Question Flow (Q1→Q2→Q3→Q4→Q5)        │
│   │   │   ├── Question Card                      │
│   │   │   ├── Answer Selection                   │
│   │   │   ├── Check Answer → Feedback            │
│   │   │   └── Next Question                      │
│   │   ├── Results Screen                         │
│   │   │   ├── Score (X/5)                        │
│   │   │   ├── Mastery Bars (per topic)           │
│   │   │   ├── Weak Topics Highlighted            │
│   │   │   ├── "Study More" → Summary Tab         │
│   │   │   └── "New Quiz" → New Quiz (adaptive)   │
│   │   └── Quiz History (past attempts)           │
│   │                                              │
│   │   CHAT TAB                                   │
│   │   ├── Chat History (scrollable)              │
│   │   ├── Suggestion Chips (empty state)         │
│   │   ├── Message Input + Send Button            │
│   │   ├── Assistant Response                     │
│   │   │   ├── Answer Text                        │
│   │   │   ├── Source Citations (Tier 2)          │
│   │   │   └── Follow-Up Chip (Tier 2)           │
│   │   └── Passage Cards (Tier 3)                 │
│   └───────────────────────────────────────────────┘
│
├── ⚙️ SETTINGS PAGE (/settings)
│   ├── Tier Override (Auto / Always Offline / Always Cloud)
│   ├── AI Model Download (progress bar, size info)
│   ├── Storage Management (breakdown + clear data)
│   ├── About KaalAman (version, tagline)
│   └── ← Back to Home
│
├── 📋 ONBOARDING (overlay, first visit only)
│   ├── Screen 1: "Upload your notes" (illustration)
│   ├── Screen 2: "Study anywhere — even offline" (illustration)
│   ├── Screen 3: "Your data stays on your device" (illustration)
│   ├── Skip Button (always visible)
│   └── → Home Page
│
└── 🔧 SYSTEM OVERLAYS (not pages — appear on top)
    ├── Upload Modal (file picker + processing progress)
    ├── Tier Transition Toast (bottom, auto-dismiss 3s)
    ├── Delete Confirmation Dialog
    ├── PWA Install Prompt (banner, dismissible)
    └── Error Toast (bottom, auto-dismiss 5s)
```

---

## 2. Complete Page Specifications

### 2.1 🏠 Home Page (`/`)

**Purpose:** Document library — the student's study hub.

```
┌──────────────────────────────────────┐
│  ☰  KaalAman              🟢  ⚙️    │ ← Header: logo, tier badge, settings
├──────────────────────────────────────┤
│                                      │
│  Your Documents                      │ ← Section title (H2)
│                                      │
│  ┌──────────────────────────────┐    │
│  │ 📄 Biology Chapter 5    🔵  │    │ ← Document card
│  │ 10 pages · 2 hours ago      │    │
│  │ Last: Quiz 3/5              │    │
│  └──────────────────────────────┘    │
│                                      │
│  ┌──────────────────────────────┐    │
│  │ 📄 Philippine History   🟡  │    │
│  │ 8 pages · Yesterday         │    │
│  │ Last: Summary viewed        │    │
│  └──────────────────────────────┘    │
│                                      │
│  ┌──────────────────────────────┐    │
│  │ 📄 Calculus Notes       🟢  │    │
│  │ 15 pages · 3 days ago       │    │
│  │ Last: Chat (5 messages)     │    │
│  └──────────────────────────────┘    │
│                                      │
│                                      │
│                          ┌────────┐  │
│                          │ + PDF  │  │ ← FAB (floating action button)
│                          └────────┘  │
└──────────────────────────────────────┘
```

**Components on this page:**

| Component | Source | Interaction |
|-----------|--------|-------------|
| `Header` | `layout/Header.jsx` | Shows app name, global tier badge, settings icon |
| `DocumentCard` | `upload/DocumentCard.jsx` | Tap → navigates to `/doc/:id/summary` |
| `DocumentCard` (swipe) | same | Swipe left → delete confirmation |
| `UploadFAB` | `upload/UploadButton.jsx` | Tap → opens file picker → processing modal |
| `EmptyState` | `common/EmptyState.jsx` | Shown when 0 documents uploaded |
| `ProcessingModal` | `upload/ProcessingBar.jsx` | Overlay during PDF extraction + embedding |

**Empty State (first visit, 0 documents):**

```
┌──────────────────────────────────────┐
│  ☰  KaalAman              🟡  ⚙️    │
├──────────────────────────────────────┤
│                                      │
│                                      │
│              📄                      │
│                                      │
│     Upload your first PDF            │
│     to get started                   │
│                                      │
│     ┌──────────────────────┐         │
│     │     Upload PDF       │         │ ← Primary button (not FAB)
│     └──────────────────────┘         │
│                                      │
│     Your notes. Your device.         │
│     Your pace.                       │ ← Tagline in gray-500
│                                      │
└──────────────────────────────────────┘
```

**Connections FROM this page:**

| Action | Destination |
|--------|-------------|
| Tap document card | → Document Page (`/doc/:id/summary`) |
| Tap ⚙️ settings icon | → Settings Page (`/settings`) |
| Tap Upload FAB / button | → Upload Modal (overlay) |
| Complete upload | → Document Page (`/doc/:id/summary`) for new doc |

---

### 2.2 📄 Document Page (`/doc/:id`)

**Purpose:** The core study experience — three tabs for three study modes.

**This is ONE page with THREE tabs.** Not three separate pages. The tab bar is a bottom navigation that swaps the content area. The URL updates for deep-linking but the page shell doesn't re-render.

```
┌──────────────────────────────────────┐
│  ←  Biology Chapter 5       🔵      │ ← Header: back, title, tier badge
├──────────────────────────────────────┤
│                                      │
│  ┌──────────────────────────────┐    │
│  │                              │    │
│  │   [TAB CONTENT AREA]        │    │
│  │                              │    │
│  │   Swappable based on        │    │
│  │   active tab below          │    │ ← Scrollable content area
│  │                              │    │
│  │                              │    │
│  │                              │    │
│  │                              │    │
│  │                              │    │
│  │                              │    │
│  └──────────────────────────────┘    │
│                                      │
├──────────────────────────────────────┤
│  [ 📝 Summary ] [❓ Quiz ] [💬 Chat]│ ← Bottom tab bar (64px)
└──────────────────────────────────────┘
```

**Tab routing:**

| Tab | URL | Component | Default? |
|-----|-----|-----------|----------|
| Summary | `/doc/:id/summary` | `SummaryView.jsx` | ✅ Yes — first tab shown |
| Quiz | `/doc/:id/quiz` | `QuizView.jsx` | No |
| Chat | `/doc/:id/chat` | `ChatView.jsx` | No |

**Connections FROM this page:**

| Action | Destination |
|--------|-------------|
| Tap ← back | → Home Page (`/`) |
| Tap tier badge | → Tier info tooltip (inline, not a page) |
| Tap Summary tab | → Summary content |
| Tap Quiz tab | → Quiz content |
| Tap Chat tab | → Chat content |
| Quiz results → "Study More" | → Summary tab (same page, scrolled to weak topics) |
| Quiz results → "New Quiz" | → Quiz tab (regenerates with adaptive targeting) |
| Chat follow-up chip | → Chat tab (sends follow-up as new message) |

---

### 2.2.1 📝 Summary Tab Content

```
┌──────────────────────────────────────┐
│  ←  Biology Chapter 5       🔵      │
├──────────────────────────────────────┤
│                                      │
│  Summary  🔵 Generated by Cloud AI   │ ← Tier label
│                                      │
│  ## Overview                         │
│  Photosynthesis is the process by    │
│  which plants convert light energy   │
│  into chemical energy stored in      │
│  glucose...                          │
│                                      │
│  ## Key Concepts                     │
│  ┌──────────────────────────────┐    │
│  │ 💡 Photolysis               │    │ ← Concept card
│  │ The splitting of water       │    │
│  │ molecules using light energy │    │
│  │ Importance: High             │    │
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │ 💡 Calvin Cycle             │    │
│  │ Light-independent reactions  │    │
│  │ that fix CO₂ into glucose   │    │
│  │ Importance: High             │    │
│  └──────────────────────────────┘    │
│                                      │
│  ## Study Outline                    │
│  1. Light Reactions                  │
│     • Photolysis                     │
│     • Electron transport chain       │
│  2. Calvin Cycle                     │
│     • Carbon fixation (RuBisCO)      │
│     • G3P production                 │
│                                      │
│  ┌──────────────────────────────┐    │
│  │   🔄 Regenerate Summary     │    │ ← Secondary button
│  └──────────────────────────────┘    │
│                                      │
├──────────────────────────────────────┤
│  [ 📝 Summary ] [❓ Quiz ] [💬 Chat]│
└──────────────────────────────────────┘
```

**Tier-specific rendering:**

| Element | Tier 1 (SLM) | Tier 2 (Bedrock) | Tier 3 (RAKE) |
|---------|-------------|-----------------|---------------|
| Overview | ✅ Markdown paragraphs | ✅ Rich paragraphs | ✅ Top 3 extracted sentences |
| Key Concepts | ✅ Term + definition | ✅ Term + definition + importance | ✅ RAKE keywords (no definitions) |
| Study Outline | ✅ Numbered topics | ✅ Topics + subtopics | ✅ Keyword clusters |
| Common Mistakes | ❌ Not generated | ✅ List of pitfalls | ❌ Not generated |
| Regenerate | ✅ Available | ✅ Available | ✅ Available (same output) |

---

### 2.2.2 ❓ Quiz Tab Content

**State machine for the Quiz tab:**

```
                    ┌─────────────┐
                    │  QUIZ START  │ ← "Start Quiz" button or auto-start
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
              ┌────►│  QUESTION   │◄────┐
              │     │  (Q1-Q5)    │     │
              │     └──────┬──────┘     │
              │            │            │
              │     ┌──────▼──────┐     │
              │     │  FEEDBACK   │     │
              │     │  ✅ or ❌    │     │
              │     └──────┬──────┘     │
              │            │            │
              │     Not last Q?  Last Q?│
              └────────────┘     │      │
                                 │      │
                          ┌──────▼──────┐
                          │  RESULTS    │
                          │  Score +    │
                          │  Mastery    │
                          └──────┬──────┘
                                 │
                    ┌────────────┼────────────┐
                    │            │            │
             ┌──────▼──┐  ┌─────▼─────┐  ┌──▼──────────┐
             │ New Quiz │  │Study More │  │ Quiz History │
             │(adaptive)│  │→ Summary  │  │ (past runs)  │
             └──────────┘  └───────────┘  └─────────────┘
```

**Question Screen:**

```
┌──────────────────────────────────────┐
│  ←  Biology Chapter 5       🔵      │
├──────────────────────────────────────┤
│                                      │
│  ●  ●  ●  ○  ○                      │ ← Progress dots (Q3 of 5)
│  Q3 of 5                   Medium    │
│                                      │
│  ─────────────────────────────────── │
│                                      │
│  What is the primary function of     │
│  RuBisCO in the Calvin cycle?        │ ← Question text
│                                      │
│  ┌──────────────────────────────┐    │
│  │  ○  A) Photolysis            │    │ ← Option (unselected)
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │  ●  B) Carbon fixation       │    │ ← Option (selected — teal border)
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │  ○  C) ATP synthesis         │    │
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │  ○  D) Water splitting       │    │
│  └──────────────────────────────┘    │
│                                      │
│  ┌──────────────────────────────┐    │
│  │       Check Answer           │    │ ← Primary button
│  └──────────────────────────────┘    │
│                                      │
├──────────────────────────────────────┤
│  [ 📝 Summary ] [❓ Quiz ] [💬 Chat]│
└──────────────────────────────────────┘
```

**Results Screen:**

```
┌──────────────────────────────────────┐
│  ←  Biology Chapter 5       🔵      │
├──────────────────────────────────────┤
│                                      │
│           3 / 5                      │ ← Large score
│        Good effort!                  │ ← Celebration message
│                                      │
│  ─────────────────────────────────── │
│                                      │
│  Topic Mastery                       │
│                                      │
│  Photolysis                          │
│  ████████████████████░░░░  85%  🟢   │
│                                      │
│  Calvin Cycle                        │
│  ██████░░░░░░░░░░░░░░░░░  25%  🔴   │ ← "Needs review" label
│                                      │
│  Light Reactions                     │
│  ████████████████░░░░░░░  65%  🟡   │
│                                      │
│  C4 Plants                           │
│  ███░░░░░░░░░░░░░░░░░░░  15%  🔴   │ ← "Needs review" label
│                                      │
│  ┌──────────────────────────────┐    │
│  │       New Quiz (Adaptive)    │    │ ← Primary button
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │       Study More →           │    │ ← Secondary → Summary tab
│  └──────────────────────────────┘    │
│                                      │
├──────────────────────────────────────┤
│  [ 📝 Summary ] [❓ Quiz ] [💬 Chat]│
└──────────────────────────────────────┘
```

---

### 2.2.3 💬 Chat Tab Content

```
┌──────────────────────────────────────┐
│  ←  Biology Chapter 5       🔵      │
├──────────────────────────────────────┤
│                                      │
│  ┌──────────────────────────┐        │
│  │ What's the difference    │        │ ← User bubble (teal bg, white text)
│  │ between C4 and CAM?      │        │
│  └──────────────────────────┘        │
│                                      │
│  ┌──────────────────────────────┐    │
│  │ C4 plants spatially separate │    │ ← Assistant bubble (gray bg)
│  │ carbon fixation from the     │    │
│  │ Calvin cycle using bundle    │    │
│  │ sheath cells, while CAM      │    │
│  │ plants temporally separate   │    │
│  │ them by fixing CO₂ at night. │    │
│  │                              │    │
│  │ 📎 Source: Page 3, ¶5        │    │ ← Source citation (Tier 2)
│  │                              │    │
│  │ 💡 How do CAM plants         │    │ ← Follow-up chip (tappable)
│  │    conserve water?           │    │
│  └──────────────────────────────┘    │
│                                      │
│                                      │
│                                      │
├──────────────────────────────────────┤
│  ┌────────────────────────┐  ┌────┐  │
│  │ Ask about your notes...│  │ ➤  │  │ ← Input + send button
│  └────────────────────────┘  └────┘  │
├──────────────────────────────────────┤
│  [ 📝 Summary ] [❓ Quiz ] [💬 Chat]│
└──────────────────────────────────────┘
```

**Chat Empty State:**

```
│                                      │
│              💬                      │
│                                      │
│    Ask anything about your notes     │
│                                      │
│  ┌────────────────────────────┐      │
│  │ "What is the Calvin cycle?"│      │ ← Suggestion chip (tappable)
│  └────────────────────────────┘      │
│  ┌────────────────────────────┐      │
│  │ "Summarize page 3"        │      │
│  └────────────────────────────┘      │
│  ┌────────────────────────────┐      │
│  │ "Explain photolysis"      │      │
│  └────────────────────────────┘      │
│                                      │
```

**Tier-specific chat rendering:**

| Element | Tier 1 (SLM) | Tier 2 (Bedrock) | Tier 3 (RAKE) |
|---------|-------------|-----------------|---------------|
| Answer | ✅ Text paragraph | ✅ Text + JSON metadata | ❌ No generation |
| Source citation | "Based on your notes" | 📎 Exact quote + page | 📋 Full passage cards |
| Follow-up chip | ❌ Not generated | ✅ Tappable suggestion | ❌ Not available |
| Confidence score | ❌ Hidden | ✅ Internal (affects refusal) | ❌ N/A |
| Refusal | ✅ "Can't find this" | ✅ "Can't find this" | ✅ "No matching passages" |

---

### 2.3 ⚙️ Settings Page (`/settings`)

```
┌──────────────────────────────────────┐
│  ←  Settings                         │
├──────────────────────────────────────┤
│                                      │
│  ## Connection Mode                  │
│                                      │
│  ┌──────────────────────────────┐    │
│  │  ● Auto (recommended)       │    │ ← Radio group
│  │  ○ Always offline            │    │
│  │  ○ Always cloud              │    │
│  └──────────────────────────────┘    │
│                                      │
│  Current: 🔵 Cloud AI               │ ← Live tier indicator
│                                      │
│  ─────────────────────────────────── │
│                                      │
│  ## AI Model                         │
│                                      │
│  SmolLM2-1.7B (On-Device AI)        │
│  Status: Not downloaded              │
│  Size: ~1.0 GB                       │
│                                      │
│  ┌──────────────────────────────┐    │
│  │     Download Model           │    │ ← Primary button
│  └──────────────────────────────┘    │
│                                      │
│  ─────────────────────────────────── │
│                                      │
│  ## Storage                          │
│                                      │
│  Total used: 1.2 GB                  │
│                                      │
│  AI Model        1.0 GB  ████████░  │ ← Breakdown bars
│  Documents       150 MB  █░░░░░░░░  │
│  Cache            50 MB  ░░░░░░░░░  │
│                                      │
│  ┌──────────────────────────────┐    │
│  │     Clear All Data           │    │ ← Destructive button (red)
│  └──────────────────────────────┘    │
│                                      │
│  ─────────────────────────────────── │
│                                      │
│  ## About                            │
│                                      │
│  KaalAman v1.0.0                     │
│  "Your notes. Your device.           │
│   Your pace."                        │
│                                      │
│  Your data stays on this device.     │
│  We never collect or store your      │
│  information.                        │
│                                      │
├──────────────────────────────────────┤
│  [ Back to Home ]                    │
└──────────────────────────────────────┘
```

**Connections FROM this page:**

| Action | Destination |
|--------|-------------|
| Tap ← back | → Home Page (`/`) |
| Change connection mode | → Updates `localStorage.tierOverride`, triggers tier re-detection |
| Tap "Download Model" | → Download progress bar (inline, not a new page) |
| Tap "Clear All Data" | → Confirmation dialog → clears IndexedDB + Cache API → Home (empty state) |

---

### 2.4 📋 Onboarding (Overlay — First Visit Only)

**Not a page.** A full-screen overlay that appears once on first app open. Stored in `localStorage.onboardingComplete = true` after dismissal.

```
SCREEN 1 of 3                          SCREEN 2 of 3                          SCREEN 3 of 3
┌──────────────────────┐               ┌──────────────────────┐               ┌──────────────────────┐
│                      │               │                      │               │                      │
│                      │               │                      │               │                      │
│        📄 → 💡       │               │     📱  (no wifi)    │               │       🔒             │
│                      │               │                      │               │                      │
│  Upload your notes,  │               │  Study anywhere —    │               │  Your data stays     │
│  get instant         │               │  even offline.       │               │  on YOUR device.     │
│  summaries & quizzes │               │                      │               │                      │
│                      │               │  No Wi-Fi? No        │               │  No accounts.        │
│                      │               │  problem. KaalAman   │               │  No tracking.        │
│                      │               │  works without       │               │  No data collection. │
│                      │               │  internet.           │               │                      │
│                      │               │                      │               │                      │
│  ○ ● ○               │               │  ○ ○ ●               │               │  ○ ○ ●               │
│                      │               │                      │               │                      │
│  [ Skip ]    [ → ]   │               │  [ Skip ]    [ → ]   │               │  [ Get Started ]     │
└──────────────────────┘               └──────────────────────┘               └──────────────────────┘
```

**Rules:**
- Skip button visible on ALL screens (top-right or bottom-left)
- Swipeable left/right between screens
- "Get Started" on last screen → dismisses overlay → Home Page
- Never shows again after first dismissal
- Total time to complete: <15 seconds (3 screens × 5 seconds reading)

---

## 3. Navigation Flow Map

### 3.1 Primary User Journey (Happy Path)

```
FIRST VISIT:
  Onboarding (3 screens, skippable)
       │
       ▼
  Home Page (empty state)
       │
       ▼ tap "Upload PDF"
  Upload Modal → file picker → processing progress
       │
       ▼ processing complete
  Document Page → Summary Tab (auto-generated)
       │
       ▼ tap Quiz tab
  Quiz Tab → 5 questions → results → mastery bars
       │
       ▼ tap "New Quiz"
  Quiz Tab → 5 ADAPTIVE questions (targets weak topics)
       │
       ▼ tap Chat tab
  Chat Tab → ask question → get answer from notes
       │
       ▼ tap ← back
  Home Page (document now in list)
```

### 3.2 Returning User Journey

```
  Home Page (documents listed)
       │
       ▼ tap document card
  Document Page → Summary Tab (loaded from cache — instant)
       │
       ▼ tap Quiz tab
  Quiz Tab → adaptive quiz (remembers mastery from last session)
       │
       ▼ results → "Study More"
  Summary Tab (scrolled to weak topics section)
```

### 3.3 Offline Journey (The Money Shot for Demo)

```
  Home Page (documents already uploaded)
       │
       ▼ [INTERNET DROPS] — toast: "🟡 Switched to offline mode"
       │
       ▼ tap document card
  Document Page → Summary Tab
       │
       ▼ tap "Regenerate Summary"
  Summary generates via Tier 3 (RAKE) — <1 second
       │
       ▼ tap Quiz tab
  Quiz generates via Tier 3 (fill-in-blank + T/F) — <1 second
       │
       ▼ tap Chat tab
  Chat retrieves passages via TF-IDF — <500ms
       │
       ▼ [INTERNET RETURNS] — toast: "🔵 Switched to Cloud AI"
       │
       ▼ tap "Regenerate Summary"
  Summary generates via Tier 2 (Bedrock) — richer, better quality
```

### 3.4 Upload Journey (Detail)

```
  Home Page
       │
       ▼ tap Upload FAB / button
  ┌─────────────────────────────────┐
  │  System file picker opens       │
  │  (accepts: .pdf only)           │
  └──────────────┬──────────────────┘
                 │
       ▼ user selects PDF
  ┌─────────────────────────────────┐
  │  Processing Modal (overlay)     │
  │                                 │
  │  Step 1: "Extracting text..."   │ ← PDF.js
  │  Step 2: "Building index..."    │ ← MiniLM embeddings (or skip for Tier 3)
  │  Step 3: "Ready!"              │ ← Saved to IndexedDB
  │                                 │
  │  [████████████████████] 100%    │
  └──────────────┬──────────────────┘
                 │
       ▼ auto-navigate
  Document Page → Summary Tab (auto-generates summary)
```

---

## 4. Component-to-Page Mapping

### 4.1 Shared Components (appear on multiple pages)

| Component | File | Pages Used |
|-----------|------|------------|
| `Header` | `components/layout/Header.jsx` | Home, Document, Settings |
| `TierBadge` | `components/layout/TierBadge.jsx` | Home (header), Document (header), Settings (live indicator) |
| `Toast` | `components/common/Toast.jsx` | All pages (tier transitions, errors) |
| `EmptyState` | `components/common/EmptyState.jsx` | Home (no docs), Chat (no messages) |
| `SkeletonLoader` | `components/common/SkeletonLoader.jsx` | Summary (generating), Quiz (generating) |
| `ConfirmDialog` | `components/common/ConfirmDialog.jsx` | Home (delete doc), Settings (clear data) |

### 4.2 Page-Specific Components

| Page | Component | File |
|------|-----------|------|
| **Home** | `DocumentCard` | `components/upload/DocumentCard.jsx` |
| **Home** | `UploadFAB` | `components/upload/UploadButton.jsx` |
| **Home** | `ProcessingModal` | `components/upload/ProcessingBar.jsx` |
| **Document** | `BottomNav` | `components/layout/BottomNav.jsx` |
| **Document** | `SummaryView` | `components/summary/SummaryView.jsx` |
| **Document** | `ConceptCard` | `components/summary/ConceptCard.jsx` |
| **Document** | `QuizView` | `components/quiz/QuizView.jsx` |
| **Document** | `QuestionCard` | `components/quiz/QuestionCard.jsx` |
| **Document** | `FeedbackCard` | `components/quiz/FeedbackCard.jsx` |
| **Document** | `ResultsView` | `components/quiz/ResultsView.jsx` |
| **Document** | `MasteryBar` | `components/quiz/MasteryBar.jsx` |
| **Document** | `ChatView` | `components/chat/ChatView.jsx` |
| **Document** | `ChatBubble` | `components/chat/ChatBubble.jsx` |
| **Document** | `SourceCard` | `components/chat/SourceCard.jsx` |
| **Document** | `SuggestionChip` | `components/chat/SuggestionChip.jsx` |
| **Settings** | `TierSelector` | `components/settings/TierSelector.jsx` |
| **Settings** | `ModelDownload` | `components/settings/ModelDownload.jsx` |
| **Settings** | `StorageBreakdown` | `components/settings/StorageBreakdown.jsx` |
| **Onboarding** | `OnboardingOverlay` | `components/onboarding/OnboardingOverlay.jsx` |

---

## 5. Route Configuration

```javascript
// App.jsx — React Router v6
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <TierProvider>
        <Routes>
          {/* Home */}
          <Route path="/" element={<HomePage />} />
          
          {/* Document — with nested tab routes */}
          <Route path="/doc/:id" element={<DocumentPage />}>
            <Route index element={<Navigate to="summary" replace />} />
            <Route path="summary" element={<SummaryView />} />
            <Route path="quiz" element={<QuizView />} />
            <Route path="chat" element={<ChatView />} />
          </Route>
          
          {/* Settings */}
          <Route path="/settings" element={<SettingsPage />} />
          
          {/* Catch-all → Home */}
          <Route path="*" element={<Navigate to="/" replace />} />
        </Routes>
        
        {/* Global overlays */}
        <Toast />
        <OnboardingOverlay />
      </TierProvider>
    </BrowserRouter>
  );
}
```

**Total routes: 6** (Home, Document shell, Summary, Quiz, Chat, Settings)
**Total pages: 3** (Home, Document, Settings)
**Total overlays: 4** (Onboarding, Upload Modal, Confirm Dialog, Toast)

---

## 6. Data Flow Per Page

### What each page READS from IndexedDB:

| Page | Store | Data |
|------|-------|------|
| Home | `documents` | All documents (title, pageCount, uploadDate, lastActivity) |
| Summary | `documents` → `summaries` | Document chunks → cached summary (or generate new) |
| Quiz | `documents` → `knowledgeState` → `quizzes` | Chunks → mastery values → generate quiz → save results |
| Chat | `documents` (embeddings) → `chatHistory` | Embeddings for HNSW search → conversation history |
| Settings | `localStorage` + Cache API | Tier override, model download status, storage sizes |

### What each page WRITES to IndexedDB:

| Page | Store | Data |
|------|-------|------|
| Home (upload) | `documents` | New document (title, rawText, chunks, embeddings) |
| Home (delete) | `documents`, `summaries`, `quizzes`, `knowledgeState`, `chatHistory` | Removes ALL data for that document |
| Summary | `summaries` | Generated summary (cached per document per tier) |
| Quiz | `quizzes`, `knowledgeState` | Quiz results + updated BKT mastery values |
| Chat | `chatHistory` | New messages (user + assistant) |
| Settings | `localStorage`, Cache API | Tier override, model cache, clear all |

---

## 7. Responsive Behavior

KaalAman is **mobile-first**. Desktop is a bonus, not a target.

| Breakpoint | Behavior |
|------------|----------|
| **< 640px** (mobile) | Full-width layout, 16px padding, bottom tab bar, FAB for upload |
| **640-1024px** (tablet) | Centered content (max-width: 640px), same layout as mobile |
| **> 1024px** (desktop) | Centered content (max-width: 640px), same layout — no sidebar, no multi-column |

**Why no desktop-specific layout:**
- 100% of our target users are on mobile (survey: budget Android phones)
- Building a desktop layout wastes 1-2 hours of build time for zero judging points
- The centered mobile layout looks clean on desktop anyway
- Judges will likely test on their phones during the demo

---

## 8. Page Count Summary

| Type | Count | List |
|------|-------|------|
| **Pages** | 3 | Home, Document (with 3 tabs), Settings |
| **Tab views** | 3 | Summary, Quiz, Chat |
| **Overlays** | 4 | Onboarding, Upload Modal, Confirm Dialog, Toast |
| **Routes** | 6 | `/`, `/doc/:id`, `/doc/:id/summary`, `/doc/:id/quiz`, `/doc/:id/chat`, `/settings` |
| **Total components** | 24 | See § 4.2 for full list |

**This is intentionally minimal.** Three pages. Three tabs. That's it. No settings sub-pages, no profile page, no analytics dashboard, no social features. Every screen exists because a user story requires it.

---

## 9. Cross-Reference to User Stories

| Page / Tab | User Stories Served |
|------------|-------------------|
| Home (empty) | US-6.1 (Zero Setup) |
| Home (upload) | US-1.1 (Upload PDF), US-1.2 (Embeddings) |
| Home (list) | US-1.3 (View Documents) |
| Summary | US-2.1 (Generate Summary), US-2.2 (Tier Label) |
| Quiz (questions) | US-3.1 (Take Quiz), US-3.3 (Adaptive BKT), US-3.4 (No Paywall) |
| Quiz (results) | US-3.2 (Weak Topics) |
| Chat | US-4.1 (Ask Questions), US-4.2 (Source Citations), US-4.3 (Hallucination Guard), US-4.4 (Follow-Up) |
| Tier Badge | US-5.2 (Know What Mode), US-5.4 (Seamless Transitions) |
| Offline (all) | US-5.1 (Use Without Internet) |
| Settings | US-5.3 (Download Model), US-8.1 (Storage), US-8.2 (Tier Override) |
| Onboarding | US-6.1 (Zero Setup — skippable) |
| Lightweight | US-6.2 (No Battery/RAM Drain) |
| Privacy | US-7.1 (Local Data) |

**Every page maps to at least one user story. Every user story maps to at least one page. No orphans.**
