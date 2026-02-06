# THE FUTURE - MVP Build Plan

## What We're Building

A website that wakes people up to the end of knowledge work - and then helps them prepare for it. Opinionated, straight-talking, urgent. Not a blog. Not a news site. A tool that grabs you, shows you the evidence, and then walks you through what to do about it.

The core experience has two phases:
1. **The Wake-Up Call** - an evidence wall that makes the coming change undeniable
2. **The Action Plan** - mindset, habits, and skills to survive and thrive

Both wrapped in a chatbot that speaks with the author's voice - certain, clear, not pulling punches, but fundamentally optimistic. Action makes the anxiety disappear.

---

## Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Framework | Next.js 14 (App Router) | Fast to ship, great DX, SSR for SEO |
| Hosting | Vercel | Zero-config deployment, edge functions |
| Styling | Tailwind CSS | Rapid UI iteration |
| AI Chat | Claude API (Anthropic) | Best reasoning, long context for content library |
| Content | MDX files in repo | No CMS overhead, version controlled, easy to write |
| Email | Resend | Simple API, good free tier, works with Next.js |
| Database | Vercel Postgres (or none in v1) | Only needed if we store user data / chat history |
| Analytics | Vercel Analytics or Plausible | Privacy-friendly, simple |

---

## Site Architecture

```
/                          → Landing page (the manifesto)
/evidence                  → The Evidence Wall (wake-up call hub)
/evidence/stocks           → SaaS stock dashboard
/evidence/voices           → CEO/founder quotes & tweets
/evidence/headlines        → Curated news headlines
/blog                      → Blog index
/blog/[slug]               → Individual blog posts (MDX)
/chat                      → AI chatbot interface
/plan                      → Action plan overview (the 5 pillars)
/plan/mindset              → Pillar 3: Growth mindset & mentality
/plan/habits               → Pillar 4: Daily habits & activities
/plan/skills               → Pillar 5: Skills for the new world
/subscribe                 → Email signup (or modal/component on every page)
```

---

## Phase 1: The Foundation (Ship This First)

**Goal:** Liv can visit a URL, read the manifesto, explore the evidence wall, and chat with the bot. Email capture is live.

### 1.1 Landing Page (`/`)

The manifesto. Clean, bold, no clutter. This is the page you text to someone.

**Content structure:**
- Hero: One punchy headline + one paragraph. The thesis.
- The 5 pillars listed simply:
  1. Understand the facts
  2. Decide who you are and what you want
  3. Get your mindset right
  4. Get your habits right
  5. Learn skills for the new world
- Each pillar is a clickable card that links to its expansion area
- Email capture CTA ("Get the daily briefing")
- Entry point to the chatbot ("Ask me anything about the future")

**Design direction:** Dark background, bold typography, minimal. Think of it as a war room briefing, not a lifestyle blog. Urgency in the design. No stock photos. No fluff.

### 1.2 The Evidence Wall (`/evidence`)

The wake-up call hub. Three sub-sections:

**1.2a Stocks Dashboard (`/evidence/stocks`)**
- Curated list of 20-30 key SaaS/tech stocks
- Show current price vs 52-week high (percentage decline)
- Visual heatmap (red = down big, green = holding)
- Brief explainer: "Why this matters to you" - connecting public market drops to private company decisions, hiring freezes, layoffs
- Data source: Free stock API (Alpha Vantage, Yahoo Finance, or Financial Modeling Prep)
- Updated daily via cron job or on-demand API call

**1.2b Voices (`/evidence/voices`)**
- Curated feed of CEO/founder/VC quotes about AI replacing jobs, layoffs, not hiring
- Each entry: person's name, role, date, quote, source link
- Commentary below each: the author's take on what this means
- Initially: manually curated, stored as MDX or JSON
- Later: semi-automated with AI finding and flagging relevant posts

**1.2c Headlines (`/evidence/headlines`)**
- Curated news headlines that build the case
- Grouped by theme (layoffs, AI adoption, company restructuring)
- Brief editorial note on each: why this matters, what to read into it
- Initially: manually curated
- Later: RSS feeds + AI filtering

### 1.3 AI Chatbot (`/chat`)

Scoped to the wake-up call pillar for v1.

**What it does:**
- Answers questions about the future of work, AI displacement, what's happening in specific industries
- Speaks in the author's voice - certain, direct, optimistic but not sugarcoating
- References content from the evidence wall and blog posts
- Can engage in back-and-forth to help users understand the scale of change

**Technical approach:**
- Claude API with a detailed system prompt capturing the author's voice, perspective, and worldview
- RAG (retrieval-augmented generation) over the content library:
  - Blog posts
  - Evidence wall data
  - Curated resources
  - The author's raw notes and opinions (from CONTEXT.md and similar)
- Content embedded and stored for retrieval (could use simple keyword matching in v1, vector DB later)
- Chat history stored in browser localStorage for v1 (no backend persistence needed initially)

**System prompt will include:**
- The author's worldview and key beliefs
- Tone guidelines (straight-talking, urgent, not angry, comfortable with uncomfortable truths)
- Key facts and data points to reference
- Instructions to always ground responses in evidence
- The underlying optimism: action is the answer, trying is the best thing you can do

### 1.4 Email Capture

- Simple email signup component (appears on landing page, evidence wall, blog posts)
- Stores emails via Resend audience/contacts API
- Confirmation email on signup
- No automated drip sequence yet - that comes in Phase 2

### 1.5 Blog (`/blog`)

- MDX-powered blog
- Each post is a file in the repo: `/content/blog/post-slug.mdx`
- Blog index page showing all posts, newest first
- Categories/tags for organisation
- Initial posts to write (content creation, not code):
  - "The End of Computer Work" - the core thesis
  - "The Horse and the Car" - the Henry Ford analogy
  - "Your Boss Is Reading These Headlines" - why public market data matters to you
  - "The 10-Minute Warning" - the mindset framework
  - "What To Do When You Can't Outrun It" - action over anxiety

---

## Phase 2: The Action Plan

**Goal:** Users who've had the wake-up call can now get practical help.

### 2.1 Plan Overview (`/plan`)

- Expands on pillars 2-5 from the manifesto
- Each pillar gets its own page with deep content

### 2.2 Mindset Section (`/plan/mindset`)

- Core content on growth mindset, resilience, stoicism
- Curated podcast/video recommendations
- The "100-mile run" framework
- Daily mindset exercises or prompts

### 2.3 Habits Section (`/plan/habits`)

- Financial habits (save, invest, live frugally)
- Time investment audit (how much of your day is "horse and buggy" work?)
- Daily/weekly checklist suggestions
- Habit tracking (simple - could just be a checklist UI)

### 2.4 Skills Section (`/plan/skills`)

- Map of future-proof skills vs dying skills
- Uniquely human skills: creativity, community building, facilitation, communication, sales
- Practical resources for each skill area
- Links to curated YouTube videos and courses (from NotebookLM library)

### 2.5 Chatbot Expansion

- Extend the chatbot to career planning mode
- "Tell me about your current job" → AI analyses exposure to automation
- Helps identify skills to develop based on their background
- Suggests job pivots and learning paths
- Still in the author's voice

### 2.6 Daily Email Drip

- Automated daily/weekly email
- Mix of: one evidence item, one positive story, one action prompt
- Personalised over time based on what they've engaged with on the site
- "The (Good) News" section for optimistic counterbalance

---

## Phase 3: Personalisation & Growth

**Goal:** The experience becomes tailored to each user.

### 3.1 User Accounts

- Simple auth (email magic link)
- Save chat history server-side
- Track what content they've read
- Store their industry/job role for personalisation

### 3.2 Personalised Feed

- Landing page feed customised to their interests and industry
- AI picks up on threads from their chat conversations
- Surfaces relevant evidence and content automatically

### 3.3 Career Planning Tool

- One-time deep-dive: chat with the bot for 20-30 minutes
- Generates a personal career transition plan
- What to lean into, what to avoid
- Exportable as PDF

### 3.4 Community

- TBD - could be a Discord, a forum, or built into the site
- The "10 people, then 100, then 1000" growth vision
- Users helping users, sharing wins and progress

---

## Content Pipeline

Content is the product. The tech is just the delivery mechanism.

### Initial Content Needed (Before Launch)

| Content | Type | Priority |
|---------|------|----------|
| Core manifesto (landing page copy) | Copy | P0 |
| "The End of Computer Work" | Blog post | P0 |
| 10-15 stock tickers with explainer | Data + copy | P0 |
| 5-10 CEO/founder quotes with commentary | Curated + copy | P0 |
| 5-10 news headlines with editorial notes | Curated + copy | P0 |
| Chatbot system prompt + voice guide | Prompt engineering | P0 |
| Author's raw opinions/worldview doc (for RAG) | Reference doc | P0 |
| "The Horse and the Car" | Blog post | P1 |
| "Your Boss Is Reading These Headlines" | Blog post | P1 |
| "The 10-Minute Warning" | Blog post | P1 |
| "What To Do When You Can't Outrun It" | Blog post | P1 |
| Export from NotebookLM (curated resources) | Data migration | P1 |

### Ongoing Content

- React to weekly news (1-2 posts/comments per week)
- Add new quotes to the Voices wall as they appear
- Update stock data (automated)
- Write deeper pieces on each of the 5 pillars
- Build out the curated resource library

---

## File Structure

```
THE-FUTURE/
├── README.md
├── BUILD_PLAN.md
├── CONTEXT.md                    # Organised background context
├── RAW_THREAD.md                 # Full conversation thread
├── src/
│   ├── app/
│   │   ├── layout.tsx            # Root layout (dark theme, nav, footer)
│   │   ├── page.tsx              # Landing page / manifesto
│   │   ├── evidence/
│   │   │   ├── page.tsx          # Evidence wall hub
│   │   │   ├── stocks/
│   │   │   │   └── page.tsx      # Stock dashboard
│   │   │   ├── voices/
│   │   │   │   └── page.tsx      # CEO/founder quotes
│   │   │   └── headlines/
│   │   │       └── page.tsx      # Curated headlines
│   │   ├── blog/
│   │   │   ├── page.tsx          # Blog index
│   │   │   └── [slug]/
│   │   │       └── page.tsx      # Individual blog post
│   │   ├── chat/
│   │   │   └── page.tsx          # AI chatbot
│   │   ├── plan/
│   │   │   ├── page.tsx          # Action plan overview
│   │   │   ├── mindset/
│   │   │   ├── habits/
│   │   │   └── skills/
│   │   └── subscribe/
│   │       └── page.tsx          # Email signup
│   ├── components/
│   │   ├── Nav.tsx
│   │   ├── Footer.tsx
│   │   ├── EmailCapture.tsx
│   │   ├── StockHeatmap.tsx
│   │   ├── QuoteCard.tsx
│   │   ├── HeadlineCard.tsx
│   │   ├── ChatInterface.tsx
│   │   ├── PillarCard.tsx
│   │   └── BlogCard.tsx
│   ├── lib/
│   │   ├── claude.ts             # Claude API wrapper
│   │   ├── stocks.ts             # Stock data fetching
│   │   ├── content.ts            # MDX content loading
│   │   └── email.ts              # Resend email wrapper
│   └── content/
│       ├── blog/                 # MDX blog posts
│       ├── voices/               # Curated quotes (JSON or MDX)
│       ├── headlines/            # Curated headlines (JSON or MDX)
│       └── system-prompt.md      # Chatbot system prompt & voice guide
├── public/
│   └── ...                       # Static assets
├── tailwind.config.ts
├── next.config.ts
├── package.json
└── tsconfig.json
```

---

## Build Order (What To Code First)

### Sprint 1: Skeleton + Manifesto (Day 1)
1. `npx create-next-app` with TypeScript + Tailwind + App Router
2. Deploy empty shell to Vercel
3. Build root layout (dark theme, navigation, footer)
4. Build landing page with manifesto content
5. Email capture component (Resend integration)
6. Push live - Liv can see the manifesto

### Sprint 2: Evidence Wall (Days 2-3)
1. Stock heatmap component + stock data API integration
2. Voices page with curated quotes (start with 5-10, stored as JSON)
3. Headlines page with curated headlines (start with 5-10)
4. Evidence hub page linking all three
5. Write explainer copy connecting stocks → "why this matters to you"

### Sprint 3: Blog (Day 3-4)
1. MDX setup for blog posts
2. Blog index page
3. Blog post template
4. Write and publish first 2-3 posts
5. Link blog posts from evidence wall and manifesto

### Sprint 4: Chatbot (Days 4-5)
1. Claude API integration (server-side route)
2. Chat interface component
3. System prompt engineering (voice, perspective, knowledge base)
4. Feed content library into context/RAG
5. Chat history in localStorage
6. Rate limiting to manage API costs

### Sprint 5: Polish + Launch (Day 5-6)
1. Mobile responsive pass
2. SEO metadata (OpenGraph, Twitter cards)
3. Loading states, error handling
4. Domain setup (once pseudonym/name is decided)
5. Test with Liv
6. Iterate based on her feedback

---

## Open Items

- [ ] Pseudonym / brand name for the site
- [ ] Domain name
- [ ] Export curated resources from NotebookLM
- [ ] Write the manifesto landing page copy
- [ ] Write the chatbot system prompt
- [ ] Select initial stock tickers to track
- [ ] Curate first batch of CEO/founder quotes
- [ ] Curate first batch of headlines
- [ ] Write first 2-3 blog posts
- [ ] Design direction - dark/bold/minimal, but specifics TBD
