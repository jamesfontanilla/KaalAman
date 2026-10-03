
# Brand Guidelines v3.0
# KaalAman — Offline-First Edge-AI Study Companion

## Document Info
| Field | Value |
|-------|-------|
| Version | 3.0 (Post-Merge + Post-Roast) |
| Last Updated | October 3, 2026 |
| Status | FINAL — Locked for Build |
| Related | PRD v3.0, System Design v3.0, User Stories v3.0 |
| Design Philosophy | "If it doesn't help you study, it doesn't exist." |

---

## 1. Brand Identity

### 1.1 Name

**KaalAman**

- **Pronunciation:** kah-AHL ah-MAHN
- **Origin:** Filipino portmanteau
  - **Kaalaman** (Filipino) = "knowledge"
  - **Aman** (Filipino) = "to harvest / to reap"
  - Combined meaning: **"Harvest your knowledge"**
- **Why this name:**
  - Instantly recognizable to Filipino students — it's a real word they already know
  - Implies active learning (harvesting), not passive consumption
  - Works in both English and Filipino contexts
  - Easy to spell, easy to say, easy to remember
  - No existing app with this exact name in the Play Store

### 1.2 Tagline

**Primary:** "Your notes. Your device. Your pace."

**Secondary (for pitch):** "Upload your notes, study anywhere — even offline."

**Why these taglines:**
- Three possessives ("your, your, your") emphasize student ownership and control
- Directly addresses the top 3 survey demands: course-relevant content (notes), local-first (device), adaptive learning (pace)
- No jargon, no buzzwords — a 5th grader can understand it

### 1.3 Logo Concept

```
┌─────────────────────────────────────────┐
│                                         │
│         📄 ──▶ 💡                       │
│                                         │
│     K A A L A M A N                     │
│                                         │
│   Your notes. Your device. Your pace.   │
│                                         │
└─────────────────────────────────────────┘

Logo elements:
- Document icon (📄) transforming into a lightbulb (💡)
- Represents: notes → understanding
- Simple enough to render at 16px favicon size
- No 3D, no gradients, no animation — flat and clean
```

**Logo Rules:**
- Always use the flat version — never add shadows, gradients, or 3D effects
- Minimum size: 32px height (for mobile app icon)
- Clear space: at least 1x the height of the "K" on all sides
- Logo works on both light and dark backgrounds

---

## 2. Target Persona — Design For "Maria"

Every design decision is filtered through one person:

| Attribute | Maria's Reality |
|-----------|----------------|
| **Age** | 20, 3rd year college |
| **Location** | Quezon City, Philippines |
| **University** | State university (SUC) — tuition is free, but everything else costs money |
| **Device** | 3-year-old Samsung Galaxy A13 (4GB RAM, 64GB storage, 40GB used) |
| **Internet** | Prepaid Globe SIM, ₱50 load lasts 3 days; campus Wi-Fi when available |
| **Monthly budget for apps** | ₱0 — she uses exclusively free-tier tools |
| **Study pattern** | Studies at night in her dorm, often after Wi-Fi hours end at 10 PM |
| **Current tools** | Free ChatGPT (hits limits), Google Docs, physical notebook, shared Quizlet |
| **Biggest frustration** | "I was already so locked in and then it tells me I have to pay" |
| **What she wants** | "I want it automatic — if I input something, everything will sort out" |

**Design filter question:** *"Would Maria understand this in 3 seconds on her Galaxy A13 at 11 PM with no Wi-Fi?"*

If the answer is no, cut it.

---

## 3. Color System

### 3.1 Design Principle: "Light, Clean, Trustworthy"

Our students told us what they hate:
> *"3D avatars, streaks, unnecessary animations"* — 15+ respondents (Q14)
> *"Yes complex dashboard… we want it simple"* — Respondent #18 (Q14)
> *"The visual effects that make the app take longer to do stuff"* — Respondent #25 (Q14)

So our palette is deliberately **calm, minimal, and functional** — the opposite of gamified EdTech.

### 3.2 Primary Palette

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| **Primary** | Deep Teal | `#0D7377` | Headers, primary buttons, active states, links |
| **Primary Light** | Soft Teal | `#14919B` | Hover states, secondary emphasis |
| **Primary Dark** | Dark Teal | `#0A5C5F` | Pressed states, text on light backgrounds |

**Why teal:**
- Teal conveys trust, calm, and intelligence — appropriate for education
- High contrast against white backgrounds (WCAG AA compliant)
- Distinct from competitor colors: Quizlet (blue/purple), Duolingo (green), ChatGPT (black/green)
- Works well on AMOLED screens (common on budget Samsung phones)

### 3.3 Tier Indicator Colors

| Tier | Color | Hex | Emoji | Meaning |
|------|-------|-----|-------|---------|
| Tier 1: On-Device AI | Green | `#16A34A` | 🟢 | "Full power, fully local" |
| Tier 2: Cloud AI | Blue | `#2563EB` | 🔵 | "Connected, best quality" |
| Tier 3: Offline Fallback | Amber | `#D97706` | 🟡 | "Basic mode, always works" |

**Why these specific colors:**
- Traffic light metaphor (green = best, amber = caution) is universally understood
- Amber (not red) for Tier 3 because it's NOT an error — it's a feature. Tier 3 always works.
- Never use red for tier indicators — red implies failure, but offline mode is intentional

### 3.4 Semantic Colors

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| **Success** | Green | `#16A34A` | Correct answers, completed actions |
| **Error** | Red | `#DC2626` | Wrong answers, system errors |
| **Warning** | Amber | `#D97706` | Low storage, connectivity warnings |
| **Info** | Blue | `#2563EB` | Tips, suggestions, follow-up chips |

### 3.5 Mastery Colors (Quiz Results)

| Mastery Level | Color | Hex | Label |
|---------------|-------|-----|-------|
| Low (<40%) | Red | `#DC2626` | 🔴 "Needs review" |
| Medium (40-70%) | Amber | `#D97706` | 🟡 "Getting there" |
| High (>70%) | Green | `#16A34A` | 🟢 "Strong" |

### 3.6 Neutral Palette

| Role | Color | Hex | Usage |
|------|-------|-----|-------|
| **Background** | White | `#FFFFFF` | Main app background |
| **Surface** | Light Gray | `#F9FAFB` | Cards, input fields, secondary surfaces |
| **Border** | Gray 200 | `#E5E7EB` | Card borders, dividers |
| **Text Primary** | Gray 900 | `#111827` | Headings, body text |
| **Text Secondary** | Gray 500 | `#6B7280` | Captions, timestamps, metadata |
| **Text Disabled** | Gray 300 | `#D1D5DB` | Disabled buttons, placeholder text |

### 3.7 Dark Mode (Future — Not for MVP)

Not building dark mode tonight. Rationale:
- Adds complexity to every component
- Budget AMOLED screens handle light mode fine
- Can be added post-hackathon with Tailwind's `dark:` prefix

---

## 4. Typography

### 4.1 Font Stack

```css
/* Primary — UI text */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;

/* Monospace — code blocks, technical content */
font-family: 'JetBrains Mono', 'Fira Code', 'Courier New', monospace;
```

**Why Inter:**
- Free, open-source (Google Fonts / bundled)
- Designed specifically for screens — excellent legibility at small sizes
- Variable font = one file, all weights (saves bandwidth)
- Excellent support for Filipino diacritical marks (ñ, etc.)
- Already cached on most Android devices (used by many apps)

### 4.2 Type Scale

| Element | Size | Weight | Line Height | Usage |
|---------|------|--------|-------------|-------|
| **H1** | 24px / 1.5rem | 700 (Bold) | 1.3 | Screen titles ("Biology Ch. 5") |
| **H2** | 20px / 1.25rem | 600 (Semibold) | 1.35 | Section headers ("Key Concepts") |
| **H3** | 16px / 1rem | 600 (Semibold) | 1.4 | Subsection headers, card titles |
| **Body** | 16px / 1rem | 400 (Regular) | 1.6 | Paragraphs, summaries, chat messages |
| **Body Small** | 14px / 0.875rem | 400 (Regular) | 1.5 | Captions, metadata, timestamps |
| **Label** | 12px / 0.75rem | 500 (Medium) | 1.4 | Badges, tags, tier indicators |
| **Button** | 16px / 1rem | 600 (Semibold) | 1.0 | Button text |

**Critical rule:** Body text is NEVER smaller than 14px. Maria is reading on a 6.6" screen at arm's length. Tiny text = instant abandonment.

### 4.3 Tailwind Config

```javascript
// tailwind.config.js — Typography
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        sans: ['Inter', ...defaultTheme.fontFamily.sans],
        mono: ['JetBrains Mono', ...defaultTheme.fontFamily.mono],
      },
      fontSize: {
        'h1': ['1.5rem', { lineHeight: '1.3', fontWeight: '700' }],
        'h2': ['1.25rem', { lineHeight: '1.35', fontWeight: '600' }],
        'h3': ['1rem', { lineHeight: '1.4', fontWeight: '600' }],
        'body': ['1rem', { lineHeight: '1.6', fontWeight: '400' }],
        'body-sm': ['0.875rem', { lineHeight: '1.5', fontWeight: '400' }],
        'label': ['0.75rem', { lineHeight: '1.4', fontWeight: '500' }],
      },
    },
  },
};
```

---

## 5. Spacing & Layout

### 5.1 Spacing Scale (4px base)

| Token | Value | Usage |
|-------|-------|-------|
| `space-1` | 4px | Inline spacing, icon gaps |
| `space-2` | 8px | Tight padding (badges, tags) |
| `space-3` | 12px | Input padding, list item gaps |
| `space-4` | 16px | Card padding, section gaps |
| `space-5` | 20px | Between sections |
| `space-6` | 24px | Screen padding (left/right) |
| `space-8` | 32px | Between major sections |

### 5.2 Layout Rules

| Rule | Value | Rationale |
|------|-------|-----------|
| **Screen padding** | 16px (left/right) | Standard mobile padding; enough for thumb reach |
| **Card padding** | 16px all sides | Comfortable reading space |
| **Card border radius** | 12px | Soft, modern, matches Android Material 3 |
| **Card shadow** | `0 1px 3px rgba(0,0,0,0.1)` | Subtle depth, no heavy shadows |
| **Button height** | 48px minimum | Google's recommended touch target size |
| **Touch target** | 44px × 44px minimum | WCAG 2.5.5 compliance |
| **Max content width** | 640px | Readable line length; centered on tablets |
| **Bottom nav height** | 64px | Room for 3-4 tabs + labels |

### 5.3 Mobile-First Grid

```
┌──────────────────────────────┐
│  Status Bar (system)         │
├──────────────────────────────┤
│  App Header (56px)           │
│  [← Back]  Title  [🟢 Tier] │
├──────────────────────────────┤
│                              │
│  Content Area                │
│  (scrollable)                │
│                              │
│  padding: 16px               │
│  max-width: 640px            │
│  margin: 0 auto              │
│                              │
│                              │
│                              │
├──────────────────────────────┤
│  Bottom Tab Bar (64px)       │
│  [Summary] [Quiz] [Chat]    │
└──────────────────────────────┘
```

---

## 6. Component Library

### 6.1 Buttons

```
PRIMARY BUTTON (main actions)
┌─────────────────────────────┐
│  Upload PDF                 │  bg: #0D7377, text: white
│                             │  height: 48px, radius: 12px
│                             │  font: 16px/600 Inter
└─────────────────────────────┘
States: hover (#14919B), pressed (#0A5C5F), disabled (gray-300)

SECONDARY BUTTON (secondary actions)
┌─────────────────────────────┐
│  New Quiz                   │  bg: white, border: #0D7377
│                             │  text: #0D7377
└─────────────────────────────┘

GHOST BUTTON (tertiary actions)
┌─────────────────────────────┐
│  Skip                       │  bg: transparent
│                             │  text: #6B7280
└─────────────────────────────┘
```

### 6.2 Cards

```
DOCUMENT CARD
┌─────────────────────────────────────┐
│  📄 Biology Chapter 5          🔵  │  ← Tier badge
│  10 pages · Uploaded 2 hours ago    │  ← Metadata in gray-500
│  Last studied: Quiz (3/5)           │  ← Last activity
└─────────────────────────────────────┘
bg: #F9FAFB, border: #E5E7EB, radius: 12px, padding: 16px
Shadow: 0 1px 3px rgba(0,0,0,0.1)
```

### 6.3 Tier Badge

```
ON-DEVICE AI          CLOUD AI            OFFLINE MODE
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 🟢 On-Device │    │ 🔵 Cloud AI  │    │ 🟡 Offline   │
└──────────────┘    └──────────────┘    └──────────────┘
bg: green-50         bg: blue-50          bg: amber-50
text: green-700      text: blue-700       text: amber-700
border: green-200    border: blue-200     border: amber-200
font: 12px/500       radius: 9999px (pill shape)
```

### 6.4 Quiz Question Card

```
QUESTION CARD (unanswered)
┌─────────────────────────────────────┐
│  Q2 of 5                    Medium  │  ← Progress + difficulty
│─────────────────────────────────────│
│  What is the primary function of    │
│  RuBisCO in the Calvin cycle?       │
│                                     │
│  ┌─────────────────────────────┐    │
│  │ ○  A) Photolysis            │    │  ← Unselected option
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ ● B) Carbon fixation        │    │  ← Selected option (teal border)
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ ○  C) ATP synthesis         │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ ○  D) Water splitting       │    │
│  └─────────────────────────────┘    │
│                                     │
│  ┌─────────────────────────────┐    │
│  │      Check Answer           │    │  ← Primary button
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘

QUESTION CARD (answered — correct)
┌─────────────────────────────────────┐
│  ✅ Correct!                        │  ← Green banner
│─────────────────────────────────────│
│  B) Carbon fixation is correct.     │
│                                     │
│  RuBisCO catalyzes the first step   │
│  of the Calvin cycle by fixing CO₂  │
│  into an organic molecule.          │  ← Explanation
│                                     │
│  ┌─────────────────────────────┐    │
│  │      Next Question →        │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘

QUESTION CARD (answered — incorrect)
┌─────────────────────────────────────┐
│  ❌ Not quite                       │  ← Red banner
│─────────────────────────────────────│
│  The correct answer is B) Carbon    │
│  fixation.                          │
│                                     │
│  You chose A) Photolysis — this     │
│  actually refers to the splitting   │
│  of water molecules in the light    │
│  reactions, not the Calvin cycle.   │  ← Distractor explanation
│                                     │
│  ┌─────────────────────────────┐    │
│  │      Next Question →        │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

### 6.5 Chat Bubble

```
USER MESSAGE
                    ┌─────────────────────────┐
                    │  What's the difference   │
                    │  between C4 and CAM      │
                    │  plants?                 │
                    └─────────────────────────┘
                    bg: #0D7377, text: white, radius: 12px 12px 0 12px

ASSISTANT MESSAGE (Tier 2)
┌─────────────────────────────────────┐
│  C4 plants spatially separate       │
│  carbon fixation from the Calvin    │
│  cycle using bundle sheath cells,   │
│  while CAM plants temporally        │
│  separate them by fixing CO₂ at     │
│  night...                           │
│                                     │
│  📎 Source: Page 3, paragraph 5     │  ← Source citation
│                                     │
│  💡 How do CAM plants conserve      │  ← Follow-up chip
│     water?                          │
└─────────────────────────────────────┘
bg: #F9FAFB, text: #111827, radius: 12px 12px 12px 0

ASSISTANT MESSAGE (Tier 3 — passage retrieval only)
┌─────────────────────────────────────┐
│  📋 Here are the most relevant      │
│  passages from your notes:          │
│                                     │
│  ┌─────────────────────────────┐    │
│  │ 1. "C4 plants use a spatial │    │  ← Retrieved passage card
│  │ separation strategy..."     │    │
│  │ 📄 Page 3                   │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │ 2. "CAM plants open their   │    │
│  │ stomata at night..."        │    │
│  │ 📄 Page 5                   │    │
│  └─────────────────────────────┘    │
└─────────────────────────────────────┘
```

### 6.6 Mastery Bar

```
TOPIC MASTERY (Quiz Results)

Photolysis          ████████████████████░░░░  85%  🟢 Strong
Calvin cycle        ██████░░░░░░░░░░░░░░░░░░  25%  🔴 Needs review
C4 plants           ███░░░░░░░░░░░░░░░░░░░░░  15%  🔴 Needs review
Light reactions     ████████████████░░░░░░░░  65%  🟡 Getting there

Bar: height 8px, radius 4px
Track: gray-200 (#E5E7EB)
Fill: semantic color based on mastery level
```

### 6.7 Progress Bar

```
PROCESSING PROGRESS
┌─────────────────────────────────────┐
│  Extracting text...        ✓        │
│  Building study index...   ████░░   │  ← Animated fill
│                            60%      │
└─────────────────────────────────────┘
Bar: height 4px, radius 2px, fill: #0D7377

QUIZ PROGRESS
  Q1    Q2    Q3    Q4    Q5
  ●     ●     ●     ○     ○
  ✓     ✗     ✓
Filled: #0D7377, Empty: gray-300
Check/X: green/red below dots
```

### 6.8 Toast Notification

```
TIER TRANSITION TOAST
┌─────────────────────────────────────┐
│  🟡 You're offline. Switched to     │
│     basic mode.                     │
└─────────────────────────────────────┘
Position: bottom, 80px from bottom edge
bg: gray-900 (#111827), text: white
radius: 12px, padding: 12px 16px
Auto-dismiss: 3 seconds
Animation: slide up, fade out
```

### 6.9 Empty States

```
HOME — NO DOCUMENTS
┌─────────────────────────────────────┐
│                                     │
│           📄                        │
│                                     │
│    Upload your first PDF            │
│    to get started                   │
│                                     │
│    ┌─────────────────────────┐      │
│    │     Upload PDF          │      │
│    └─────────────────────────┘      │
│                                     │
└─────────────────────────────────────┘
Icon: 48px, gray-400
Text: 16px, gray-500
Button: Primary

CHAT — NO MESSAGES
┌─────────────────────────────────────┐
│                                     │
│           💬                        │
│                                     │
│    Ask anything about your notes    │
│                                     │
│    Try: "What is the Calvin cycle?" │  ← Suggestion chip
│    Try: "Summarize page 3"         │
│                                     │
└─────────────────────────────────────┘
```

---

## 7. Iconography

### 7.1 Icon Style

- **Source:** Heroicons (by Tailwind Labs) — outline style
- **Why:** Free, MIT licensed, designed for Tailwind, consistent stroke width
- **Size:** 24px default, 20px in compact contexts, 16px inline with text
- **Stroke:** 1.5px (outline variant)
- **Color:** Inherits text color (currentColor)

### 7.2 Icon Map

| Context | Icon | Heroicon Name |
|---------|------|---------------|
| Upload | 📄 | `document-arrow-up` |
| Summary | 📝 | `document-text` |
| Quiz | ❓ | `academic-cap` |
| Chat | 💬 | `chat-bubble-left-right` |
| Settings | ⚙️ | `cog-6-tooth` |
| Back | ← | `arrow-left` |
| Delete | 🗑️ | `trash` |
| Correct | ✅ | `check-circle` (solid, green) |
| Incorrect | ❌ | `x-circle` (solid, red) |
| Info | ℹ️ | `information-circle` |
| Source | 📎 | `paper-clip` |
| Follow-up | 💡 | `light-bulb` |
| Download | ⬇️ | `arrow-down-tray` |
| Storage | 💾 | `circle-stack` |

---

## 8. Voice & Tone

### 8.1 Brand Voice Principles

| Principle | What It Means | Example |
|-----------|--------------|---------|
| **Friendly** | Talk like a helpful classmate, not a professor | "Here's what your notes say about photosynthesis" not "The following is a comprehensive analysis of..." |
| **Direct** | Say it in one sentence, not three | "Upload a PDF to start" not "To begin your learning journey, please select a document from your device's file system" |
| **Honest** | Admit limitations openly | "I can't find this in your notes" not silence or hallucination |
| **Filipino-aware** | Understand the context without being patronizing | Use "₱" not "$"; reference "jeepney" not "commute"; never explain what a "blockmate" is |

### 8.2 Writing Rules

| Rule | Do ✅ | Don't ❌ |
|------|-------|---------|
| **Short sentences** | "Your summary is ready." | "We have successfully generated a comprehensive summary of your uploaded document." |
| **Active voice** | "We found 3 relevant passages." | "3 relevant passages were found in your document." |
| **No jargon** | "Basic mode" | "Deterministic fallback tier" |
| **No corporate speak** | "This works offline." | "Leveraging edge computing for offline-first experiences." |
| **Contractions** | "Can't find this in your notes." | "Cannot locate the requested information." |
| **Filipino-English mix OK** | "Your kodigo is ready!" | Forced pure English when Taglish is natural |

### 8.3 Tier Naming (User-Facing)

| Internal Name | User-Facing Name | Description Shown |
|---------------|-----------------|-------------------|
| Tier 1: Edge-AI SLM | 🟢 **On-Device AI** | "AI running on your device — no internet needed" |
| Tier 2: Cloud-AI Bedrock | 🔵 **Cloud AI** | "Connected to the cloud — best quality" |
| Tier 3: Deterministic Fallback | 🟡 **Offline Mode** | "Basic mode — always works, no internet needed" |

**Never say:** "Tier 1", "Tier 2", "Tier 3", "SLM", "Bedrock", "RAKE", "deterministic", "fallback", "edge inference"

### 8.4 Error Messages

| Situation | Message |
|-----------|---------|
| PDF has no text | "This PDF looks like a scanned image. Try a text-based PDF instead." |
| PDF too large | "This PDF is too large (over 50 pages). Try uploading individual chapters." |
| Chat can't find answer | "I can't find this in your notes. Try asking about a topic that's in your document." |
| Tier 2 API fails | *(silent — falls to Tier 1 or 3, shows toast)* "Switched to offline mode." |
| Storage full | "Your device is running low on space. Delete some old documents in Settings." |
| Model download fails | "Download interrupted. You can try again when you have a stable connection." |
| WASM not supported | *(silent — Tier 1 disabled, uses Tier 2 or 3)* |

### 8.5 Celebration Messages (Quiz Results)

| Score | Message |
|-------|---------|
| 5/5 | "Perfect! 🎉 You've mastered this material." |
| 4/5 | "Almost there! One topic needs a quick review." |
| 3/5 | "Good effort! Let's strengthen those weak spots." |
| 2/5 | "Keep going — review the highlighted topics and try again." |
| 1/5 | "Tough one. Let's go back to the summary and review." |
| 0/5 | "No worries — everyone starts somewhere. Check the summary first." |

---

## 9. Anti-Patterns — What We Will NEVER Build

Grounded directly in survey bloatware complaints (Q14):

| Anti-Pattern | Survey Evidence | Our Rule |
|--------------|----------------|----------|
| **3D Avatars** | 15+ respondents explicitly named this as bloatware | No avatars, no mascots, no characters |
| **Social Feeds** | "Social feeds — I'm using the app for study, not to scroll" (R#12) | No social features, no feeds, no sharing |
| **Complex Dashboards** | "Yes complex dashboard… we want it simple" (R#18) | Maximum 3 tabs per screen |
| **Gamification / Streaks** | "3D avatars, streaks, unnecessary animations" (R#5) | No streaks, no XP, no leaderboards, no badges |
| **Unnecessary Animations** | "The visual effects that make the app take longer to do stuff" (R#25) | Only functional animations (loading, transitions) |
| **Ads** | "Definitely ads, especially the ones where any accidental tap redirects you" (R#15) | Zero ads, ever |
| **Forced Sign-Up** | 20/40 selected "Zero account setup" as god-tier (Q13) | No accounts, no email, no sign-up |
| **Subscription Nag** | "It demands money from the user even after using it for like 20 mins" (R#1) | No "upgrade" popups, no usage limits on Tier 1/3 |
| **Heavy Animations** | "Interactive animation, waste of time and RAM" (R#4) | CSS transitions only, no JS animation libraries |

---

## 10. Accessibility

### 10.1 Minimum Standards

| Standard | Target |
|----------|--------|
| **Color contrast** | WCAG AA (4.5:1 for text, 3:1 for large text) |
| **Touch targets** | 44px × 44px minimum |
| **Font size** | Never below 14px for body text |
| **Focus indicators** | Visible focus ring on all interactive elements |
| **Screen reader** | Semantic HTML (proper headings, labels, ARIA where needed) |
| **Motion** | Respect `prefers-reduced-motion` — disable all animations |

### 10.2 Contrast Verification

| Combination | Ratio | Pass? |
|-------------|-------|-------|
| Primary (#0D7377) on White (#FFFFFF) | 5.2:1 | ✅ AA |
| Gray-900 (#111827) on White (#FFFFFF) | 17.4:1 | ✅ AAA |
| Gray-500 (#6B7280) on White (#FFFFFF) | 4.6:1 | ✅ AA |
| White on Primary (#0D7377) | 5.2:1 | ✅ AA |
| Green-700 on Green-50 | 5.1:1 | ✅ AA |
| Amber-700 on Amber-50 | 4.9:1 | ✅ AA |
| Blue-700 on Blue-50 | 5.3:1 | ✅ AA |

---

## 11. Tailwind Configuration (Complete)

```javascript
// tailwind.config.js
const defaultTheme = require('tailwindcss/defaultTheme');

module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        // Brand primary
        primary: {
          50:  '#F0FDFA',
          100: '#CCFBF1',
          200: '#99F6E4',
          300: '#5EEAD4',
          400: '#2DD4BF',
          500: '#14919B',  // Primary light
          600: '#0D7377',  // Primary (main)
          700: '#0A5C5F',  // Primary dark
          800: '#084547',
          900: '#062E2F',
        },
        // Tier indicators
        tier: {
          edge:  '#16A34A',  // Tier 1 — green
          cloud: '#2563EB',  // Tier 2 — blue
          offline: '#D97706', // Tier 3 — amber
        },
        // Mastery levels
        mastery: {
          low:    '#DC2626',  // <40%
          medium: '#D97706',  // 40-70%
          high:   '#16A34A',  // >70%
        },
      },
      fontFamily: {
        sans: ['Inter', ...defaultTheme.fontFamily.sans],
        mono: ['JetBrains Mono', ...defaultTheme.fontFamily.mono],
      },
      borderRadius: {
        'card': '12px',
        'button': '12px',
        'badge': '9999px',
        'input': '8px',
      },
      spacing: {
        'header': '56px',
        'bottom-nav': '64px',
        'touch': '44px',
      },
      maxWidth: {
        'content': '640px',
      },
      boxShadow: {
        'card': '0 1px 3px rgba(0, 0, 0, 0.1)',
        'card-hover': '0 4px 6px rgba(0, 0, 0, 0.1)',
      },
    },
  },
  plugins: [],
};
```

---

## 12. App Icon & PWA Manifest

### 12.1 App Icon Specifications

| Size | Usage |
|------|-------|
| 192 × 192 | Android home screen |
| 512 × 512 | Android splash screen, Play Store |
| 180 × 180 | iOS home screen (apple-touch-icon) |
| 32 × 32 | Browser favicon |
| 16 × 16 | Browser tab favicon |

**Icon design:**
- Background: Primary (#0D7377)
- Foreground: White document-to-lightbulb icon
- Safe zone: 66% of icon area (for Android adaptive icons)
- No text in icon — just the symbol

### 12.2 PWA Manifest

```json
{
  "name": "KaalAman — Study Companion",
  "short_name": "KaalAman",
  "description": "Your notes. Your device. Your pace.",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#FFFFFF",
  "theme_color": "#0D7377",
  "orientation": "portrait",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png" },
    { "src": "/icons/icon-512-maskable.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ],
  "categories": ["education"],
  "lang": "en",
  "dir": "ltr"
}
```

---

## 13. Design Decision Log

| Decision | Rationale | Survey Evidence |
|----------|-----------|-----------------|
| No dark mode (MVP) | Reduces build complexity by ~30% | Not requested in survey |
| Teal primary, not blue | Differentiates from Quizlet/ChatGPT; conveys calm trust | — |
| Amber for Tier 3, not red | Offline mode is a feature, not an error | 34/40 chose offline option (Q12) |
| No animations beyond transitions | Students hate "visual effects that slow things down" | R#25 (Q14) |
| Inter font, not custom | Already on most Android devices; saves download | — |
| 48px button height | Google Material 3 recommended touch target | — |
| 16px minimum body text | Readability on 5-6" budget phone screens | — |
| No onboarding required | 50% selected "zero setup" as god-tier | 20/40 (Q13) |
| Pill-shaped tier badges | Instantly scannable; traffic-light metaphor | — |
| Flat design, no shadows on text | Reduces visual noise; faster rendering on budget GPUs | "We want it simple" R#18 (Q14) |
