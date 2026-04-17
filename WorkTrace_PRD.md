# WorkTrace — Product Requirements Document

**Version:** 1.1  
**Date:** April 2026  
**Status:** Approved for Development  
**Author:** Product Team  
**Audience:** Engineering, Design, QA, Leadership

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Solution Overview](#3-solution-overview)
4. [Goals & Success Metrics](#4-goals--success-metrics)
5. [User Persona](#5-user-persona)
6. [Product Principles](#6-product-principles)
7. [Feature Specification](#7-feature-specification)
8. [Meeting Action Items](#8-meeting-action-items)
9. [Data Model](#9-data-model)
10. [Smriti — AI Assistant Design](#10-smriti--ai-assistant-design)
11. [UI/UX Design](#11-uiux-design)
12. [Technical Architecture](#12-technical-architecture)
13. [Phased Roadmap](#13-phased-roadmap)
14. [Build Approach](#14-build-approach)
15. [Testing Strategy](#15-testing-strategy)
16. [Publishing & Distribution](#16-publishing--distribution)
17. [Open Questions & Risks](#17-open-questions--risks)

---

## 1. Executive Summary

**WorkTrace** is a personal AI-powered work journal for software developers. It solves a real and widespread problem: developers do significant, high-impact work throughout the year — production incident fixes, feature delivery, system integrations, mentoring, architecture solutioning, deployments — but fail to document or recall most of it at appraisal time.

WorkTrace addresses this through **Smriti**, a personal AI assistant (named after the Sanskrit word for *memory/remembrance*) who proactively reaches out every evening, has a natural conversation about the developer's day, and silently builds a structured, searchable, and reportable record of all their work.

The interaction model is **conversational, not form-based**. The developer never opens a dashboard and fills fields. They talk to Smriti like a colleague. She asks, listens, remembers, and at appraisal time — the work is already documented.

---

## 2. Problem Statement

### The Invisible Work Problem

Software developers perform work across a wide range of categories that rarely get documented:

| Category | Examples |
|----------|---------|
| Feature Development | New features, enhancements, refactors |
| Integrations | Third-party APIs, payment gateways, services |
| Bug Fixes / Production Issues | Hotfixes, root-cause analysis, incident response |
| Onboarding / Mentoring | Helping new team members ramp up |
| Solutioning / Architecture | Design discussions, technical proposals, spike work |
| Audits | Code reviews, security audits, compliance checks |
| Deployments / Script Runs | Production deployments, DB migrations, cron jobs |
| Documentation | Writing specs, runbooks, API docs |
| Meetings / Discussions | Cross-team alignment, sprint planning, retrospectives |
| Learning / Research | Evaluating libraries, studying new tech |

At appraisal time, the developer is asked: *"What did you accomplish this year?"*

The honest answer is: *a lot* — but none of it is accessible. Jira only captures formally ticketed work. Most of the above never makes it into Jira. The result is that developers undersell themselves, key contributions go unrecognised, and appraisal outcomes are driven by recency bias rather than actual impact.

### Why Existing Tools Don't Solve This

| Tool | Why it fails |
|------|-------------|
| Jira / Linear | Only captures ticketed work; requires manual logging; no personal narrative |
| Notion / Docs | Too much friction; not designed for daily micro-logging |
| Notes apps | Unstructured; not searchable by category or date; no reporting |
| Memory | Unreliable; recency-biased; stressful to reconstruct |

---

## 3. Solution Overview

WorkTrace is an **Android-first mobile app** (iOS later) with the following core loop:

```
Daily notification (8 PM)
        ↓
Smriti greets the developer conversationally
        ↓
Developer describes their day in natural language
        ↓
Smriti extracts tasks, fuzzy-matches against history
        ↓
Confirmation: update existing task OR create new one
        ↓
Open tasks from previous days reviewed & updated
        ↓
Session summary saved → all data stored locally on device
        ↓
Anytime: ask Smriti anything, get instant recall + charts
        ↓
Appraisal time: generate PDF summary, export, done.
```

All data is stored **locally on device**. No backend, no cloud, no subscription in v1. Privacy-first.

---

## 4. Goals & Success Metrics

### Primary Goals
- Developer logs work daily with less than 3 minutes of effort
- Zero tasks are lost or forgotten over a 12-month cycle
- Appraisal preparation time reduces from hours to minutes

### Success Metrics (v1)

| Metric | Target |
|--------|--------|
| Daily check-in completion rate | > 70% of weekdays |
| Avg time per daily check-in session | < 3 minutes |
| Tasks logged per user per month | > 20 |
| D30 retention | > 50% |
| Appraisal summary export used | > 80% of users at year-end |

### North Star Metric
> Number of tasks logged per active user per year — proxy for how much invisible work is now captured.

---

## 5. User Persona

### Primary: "The Senior Developer"

| Attribute | Detail |
|-----------|--------|
| Role | Software Engineer / Senior SWE / Tech Lead |
| Experience | 3–10 years |
| Team size | 5–20 people |
| Pain | Forgets 80% of their work by appraisal time |
| Jira usage | Uses it but knows most of their work isn't in it |
| Behaviour | Works across multiple contexts daily — code, meetings, mentoring |
| Device | Android primary, tech-comfortable |
| Motivation | Wants fair recognition, not to fight for it |

### Secondary: "The New Developer"

| Attribute | Detail |
|-----------|--------|
| Role | Junior / Mid SWE |
| Pain | Doesn't know what to log; underestimates their own contributions |
| Value from WorkTrace | Smriti helps them see the value of their own work; builds habit early |

---

## 6. Product Principles

1. **Conversation first.** No forms, no fields. If it feels like filling out a ticket, it's wrong.
2. **She comes to you.** The developer should never have to remember to open the app.
3. **Zero friction capture.** Logging a task should feel faster than sending a Slack message.
4. **Privacy by default.** All data on device. No cloud without explicit opt-in.
5. **Smriti has personality.** She's not a bot. She understands developer work life — the pressure, the wins, the invisible grind.
6. **Appraisal is the output.** Every design decision is evaluated against: does this make appraisal easier?
7. **Don't replicate Jira.** WorkTrace captures what Jira misses, not what Jira already does.

---

## 7. Feature Specification

### 7.1 Daily Check-in (Core Loop)

**Trigger:** Local notification at configurable time (default: 8:00 PM)

**Notification variants:**

| Context | Message |
|---------|---------|
| Regular weekday | "Hey [Name], it's Smriti 👋 What did you work on today?" |
| Friday | "End of the week! Let's wrap up and log what you've built." |
| Monday after leave | "Welcome back! You were away [N] days — I marked those as leave. Ready to catch up?" |
| Has stale open tasks | "You have [N] open tasks — any updates today?" |
| Public holiday | Skipped entirely OR light optional nudge |

**Flow:**
1. User taps notification → opens directly into Smriti chat
2. Smriti greets contextually based on day/time/history
3. User types freely in natural language
4. Smriti parses input → extracts individual task mentions
5. For each task:
   - Runs fuzzy match against all open/recent tasks in SQLite
   - If match found (confidence > threshold): shows **confirmation card**
     - **Yes → update it:** adds a new log entry to the existing task
     - **No → create new:** begins new task creation flow
   - If no match: creates new task, asks for category if unclear
6. After today's tasks: reviews open tasks from previous days
7. Session ends with brief summary: "Logged 3 tasks today. 2 still open from last week."

---

### 7.2 Task Management

#### Task Categories (11)
1. Feature Development
2. Integration
3. Bug Fix / Production Issue
4. Onboarding / Mentoring
5. Solutioning / Architecture
6. Audit
7. Deployment / Script Run
8. Documentation
9. Meeting / Discussion
10. Learning / Research
11. Other

#### Task Statuses (5)
| Status | Meaning |
|--------|---------|
| `Open` | Actively being worked on |
| `On Hold` | Blocked or paused, not abandoned |
| `In Review` | Waiting on external input (PR review, approval, etc.) |
| `Done` | Completed and closed |
| `Abandoned` | Will not be completed |

#### Task Priorities (4)
`Low` · `Medium` · `High` · `Critical`

#### Task Relationships
- **Parent-child:** A task can have sub-tasks (e.g. "UPI Integration" → "Deeplink flow", "Mandate flow")
- **Linked tasks:** Related but not hierarchical (e.g. a bug caused by a feature task)

---

### 7.3 Dashboard (My Work)

A structured view of all logged tasks with:

**Overview Stats:**
- Total tasks (this month / all time)
- Completed count + close rate %
- Open task count
- Log entries / streak

**Analytics:**
- Category breakdown — horizontal bar chart (% and count per category)
- Priority distribution — donut chart
- Tasks closed per week — bar chart (rolling 8 weeks)
- Longest open tasks — list sorted by days open, colour-coded (green < 3 days, amber 3–7, red > 7)
- Task duration heatmap — grid view, each cell = a task, colour = time to close

**Task List:**
- Filterable by: category, status, priority, date range, tags
- Sortable by: date created, date updated, priority, duration
- Each item shows: title, status badge, category badge, age

**Task Detail:**
- Full activity log (chronological daily entries)
- Linked / child tasks
- Status change actions
- Tags
- Edit any field

---

### 7.4 Insights

Period selector: `Weekly` · `Monthly` · `Quarterly` · `Annual`

**Charts & Analytics:**

| Chart | Description |
|-------|-------------|
| Velocity | Tasks completed per week, rolling 8-week bar chart |
| Category trend | Stacked bar — how category mix changed month over month |
| Avg resolution time | Per category — e.g. "Bug Fix: avg 2.3 days", "Feature Dev: avg 8.1 days" |
| Cycle comparison | This period vs previous — total tasks, categories, close rate |
| Focus score | % deep work (Feature Dev + Integration + Solutioning) vs shallow (Meetings, Misc) |
| Activity heatmap | GitHub-style — days with logged activity shown darker |

**Smriti's Narrative:**
A written paragraph generated by Smriti interpreting the analytics in plain English:
> *"Big quarter. You closed 34 tasks — up 28% from Q4. Your heaviest category was Bug Fix/Prod Issues (12 tasks), which is worth calling out: you were clearly the stability anchor for the team. Your avg resolution time for bugs dropped to 1.8 days from 3.2 — that's a genuine improvement. One thing to watch: meetings have crept up to 22% of your logged time. Consider whether that's by design or worth pushing back on."*

**Highlights & Nudges:**
- Streak milestones ("30 days logged in a row!")
- Unlogged category alerts ("You mentioned architecture discussions 4 times this month but logged 0 Solutioning tasks — want to add them?")
- Improvement callouts ("Your close rate is up 15% this quarter")

**Export:**
- PDF export of any period — formatted for copy-paste into appraisal tools
- Text export (plain bullet list)
- Share via any installed Android share target

---

### 7.5 Search & Recall (Smriti Anytime Mode)

The developer can ask Smriti any question about their work history at any time — not just during the daily check-in.

**Supported query types:**

| Type | Example query | Smriti's response |
|------|--------------|-------------------|
| Task lookup | "Did I work on UPI integration?" | Fuzzy-matched task cards with status, dates, summary |
| Aggregate | "How many prod bugs did I fix this year?" | Count + inline bar chart by month |
| Category breakdown | "What kind of work have I done most?" | Category % with bar chart |
| Time-based | "What did I do last month?" | Summary count by category + task list |
| Duration query | "What was my longest running task?" | Task name + days open |
| Appraisal prep | "Give me a summary for my appraisal" | Structured narrative + export option |
| Fuzzy / vague | "That auth thing I did for Ravi" | Best fuzzy match with full context |

**Technical flow:**
1. Gemma classifies intent: `lookup` / `aggregate` / `timeline` / `appraisal`
2. SQLite query: fuzzy match on title + description + tags + log text; GROUP BY for aggregates
3. Gemma narrates result conversationally
4. If numeric: inline mini bar chart or stat pills rendered inside chat bubble
5. Quick-reply chips: "See full list", "Open the task", "Export this", "Compare with last year"

---

### 7.6 Smriti — Emotional Intelligence

Smriti is not purely a task logger. She recognises emotional state and responds as a companion, not a bot.

**Emotion Detection Signals:**

| Signal type | Indicators |
|-------------|-----------|
| Frustration | "I'm done", "why does this always", excessive !!! or CAPS, person name + complaint |
| Exhaustion | "so tired", "back to back", "burned out", "no time to code" |
| Victory | "finally", "shipped", "it works", "got the approval", "live" |
| Anxiety | "deadline", "stuck", "don't know how", "pressure", "going to miss" |
| Team conflict | person name + negative verb, "scope creep", "last minute", "blame", "unfair" |
| Self-doubt | "not doing enough", "useless", "imposter", "behind everyone" |
| Pattern-based | Same person mentioned negatively 3x in a month; stress logged 5+ consecutive days |

**7 Response Modes:**

| Mode | Trigger | Behaviour |
|------|---------|-----------|
| **Listener** | Opening of a rant | Absorbs fully before responding. "Tell me everything." |
| **Solidarity** | Manager / team frustration, unfair blame | Takes the developer's side. Validates. Asks clarifying questions. |
| **Gentle Scold** | Self-criticism, imposter syndrome | Pushes back with specific evidence from their own task history. |
| **Soother** | Burnout, overwhelm | Calm, slow tone. Doesn't push task logging. "You don't have to solve everything today." |
| **Gentle Redirect** | Prolonged spiral (>10 min venting, no resolution) | Breaks the loop with a grounding question. Still warm. |
| **Celebrator** | Wins, shipments, fixes, approvals | Matches energy. Celebrates. Immediately ties to appraisal value. |
| **Search & Recall** | Any question about past work | Fuzzy search + conversational answer + inline chart. |

**Rules of Engagement:**
- Never pivots to task logging mid-vent — waits until the developer is ready
- Never gives generic HR-speak ("have you tried talking to your manager?")
- Remembers recurring patterns and surfaces them gently
- Calls out the developer's own work when they minimise it
- Tone always matches energy — punchy when annoyed, slow when exhausted
- All emotional responses stay on-device — nothing leaves the phone

---

### 7.7 Theme System

| Mode | Theme name | Palette |
|------|-----------|---------|
| Dark | Void Violet | Black `#0a0a0f` + Violet `#8b5cf6` accent |
| Light | Arctic Light | Off-white `#f8f7ff` + Indigo `#6366f1` accent |

**Switching logic:**
1. Default: follows system `prefers-color-scheme`
2. In-app override in Settings: `System` / `Dark` / `Light`
3. Preference persisted in `SharedPreferences`

---

## 8. Meeting Action Items

### 8.1 Overview

During meetings, developers agree on action items — tasks they need to create, decisions they need to follow up on, blockers they need to unblock. In back-to-back meeting culture, these are forgotten within minutes.

Meeting Action Items is a **lightweight, fast-capture, smart-reminder** feature built directly into Smriti's chat. It is intentionally distinct from full Tasks — action items are quick commitments that either get promoted into Tasks or closed out, with Smriti following up until one of those happens.

---

### 8.2 Action Item vs Task — Key Distinction

| Attribute | Action Item | Task |
|-----------|-------------|------|
| Created from | Meeting mention / quick chat capture | Evening check-in or manual creation |
| Detail level | Light (title + meeting context) | Full (logs, category, priority, links) |
| Lifecycle | Pending → Picked Up → Promoted / Dropped | Open → On Hold / In Review → Done |
| Reminder type | Smart: 2 hr / next morning / daily until resolved | Passive: shows in open task list |
| Jira intent | Explicit ("need to create a ticket for this") | Implicit |
| Urgency | High — meeting context implies time-sensitivity | Variable |
| Assigned to | Self or Others (dependency tracking) | Self only |

---

### 8.3 Capture Flow (Anytime)

The developer opens WorkTrace at any point during the day — mid-meeting, right after, between meetings — and tells Smriti in natural language:

> *"In the architecture sync just now I agreed to create a spike ticket for the cache layer redesign, and Priya said she'll share the infra diagram by tomorrow."*

**Smriti extracts:**

| Item | Owner | Type |
|------|-------|------|
| Create spike ticket — cache layer redesign | You | Your action item |
| Priya shares infra diagram | Priya | Dependency (tracked, not reminded) |

**Smriti responds:**

> *"Got it. Two things from the arch sync — a spike ticket you need to create, and Priya's infra diagram which should come tomorrow. When should I remind you about the spike ticket — in 2 hours, or tomorrow morning?"*

Developer picks a reminder time. Smriti schedules it. Takes 20 seconds total.

---

### 8.4 Action Item States

```
Pending
  ├─→ Picked Up → Promoted to Task (done)
  ├─→ Picked Up → Closed (done, no task needed)
  └─→ Not Yet → Snoozed (re-reminded in N hours / next morning / next day)
                    └─→ (loops until Picked Up or manually Dropped)
```

| State | Meaning |
|-------|---------|
| **Pending** | Captured, not yet acted on |
| **Snoozed** | Acknowledged, deferred to a specific time |
| **Picked Up** | Developer confirmed they've started or done it |
| **Promoted** | Converted into a full Task |
| **Closed** | Done but no Task needed (e.g. sent a message, quick action) |
| **Dropped** | Won't be done — archived with reason |

---

### 8.5 Reminder Intelligence

After capture, Smriti asks one question:

> *"When should I remind you — in 2 hours, or tomorrow morning?"*

**Reminder schedule options:**
- In 1 hour
- In 2 hours
- End of day (5 PM)
- Tomorrow morning (configurable, default 9 AM)
- Custom time

**When the reminder fires** (as a notification):

> **Smriti (2:30 PM):** "Hey — you mentioned after the arch sync that you'd create a spike ticket for the cache layer redesign. Have you picked it up?"

**Developer responds:**

**Path A — Yes, picked it up:**
> Smriti: "Want to log it as a task? Which category — Feature Dev or Solutioning?"
> → Item promoted to full Task, meeting context becomes first activity log entry

**Path B — Not yet:**
> Smriti: "No problem. Remind you again in 2 hours, or tomorrow morning?"
> → Re-scheduled. Smriti never silently drops it.

**Path C — Won't do it:**
> Smriti: "Got it — dropping it. Anything worth noting as context?"
> → Archived as Dropped with optional note

---

### 8.6 Dependency Tracking (Others' Action Items)

When the developer mentions someone else's commitment, Smriti tracks it as a **dependency** — she won't assign it as the developer's action, but she will follow up on it:

**Capture:**
> *"Priya said she'll share the infra diagram by tomorrow"*

**Next day if not mentioned:**
> Smriti (during evening check-in): *"By the way — Priya was supposed to share the infra diagram today. Did you receive it? If not, might be worth a follow-up."*

This surfaces blockers before they become blockers. The dependency is never auto-resolved — the developer manually marks it received or follows up.

---

### 8.7 Morning Digest — Fresh Start Notification

Every morning at a configurable time (default: 9:00 AM), if there are pending action items or dependencies, Smriti sends a **Fresh Start notification:**

> **Smriti (9:00 AM):** "Good morning! Before you dive in — 3 things from yesterday's meetings are waiting. Tap to go through them."

Inside the app, Smriti runs through each item conversationally:
1. *"You agreed to create the spike ticket for cache redesign — still pending. Want to pick it up now?"*
2. *"You were expecting the infra diagram from Priya — did that come through?"*
3. *"You said you'd review Ravi's PR by EOD yesterday — any update?"*

This is a distinct notification from the 8 PM evening check-in. It's the **developer's daily briefing** — what's pending, what's expected from others, what needs attention today.

---

### 8.8 Evening Check-in Integration

During the evening check-in, Smriti naturally closes the loop on any action items from that day:

> *"You had 2 action items from meetings today — the cache spike ticket and the deployment plan share. Did either of those get done?"*

- If done → promote to task or mark closed
- If not done → ask if tomorrow morning reminder works

This ensures action items never silently vanish — they surface every evening until resolved.

---

### 8.9 Action Items in the Dashboard

A dedicated **"Action Items"** section in the My Work tab:

**Three subsections:**
1. **Pending** — your unresolved action items, sorted by reminder time
2. **Waiting on others** — dependencies you're tracking
3. **Resolved this week** — recently promoted/closed items (shows velocity)

Each item shows:
- Title
- Meeting context (e.g. "From: Architecture Sync · Apr 8")
- Reminder countdown ("Reminding in 1h 20m")
- Quick actions: Pick it up · Snooze · Drop it

---

### 8.10 Data Model for Action Items

```sql
CREATE TABLE action_items (
  id              TEXT PRIMARY KEY,
  title           TEXT NOT NULL,
  meeting_context TEXT,             -- e.g. "Architecture sync, Apr 8"
  owner           TEXT DEFAULT 'self', -- 'self' or person name
  status          TEXT NOT NULL,    -- Pending|Snoozed|PickedUp|Promoted|Closed|Dropped
  promoted_task_id TEXT REFERENCES tasks(id), -- set if Promoted
  remind_at       INTEGER,          -- unix timestamp of next reminder
  reminder_count  INTEGER DEFAULT 0, -- how many times reminded so far
  notes           TEXT,             -- optional context / reason if dropped
  created_at      INTEGER NOT NULL,
  updated_at      INTEGER NOT NULL,
  resolved_at     INTEGER
);

CREATE INDEX idx_action_items_status    ON action_items(status);
CREATE INDEX idx_action_items_remind_at ON action_items(remind_at);
CREATE INDEX idx_action_items_owner     ON action_items(owner);
```

---

### 8.11 Smriti's Extraction Logic for Action Items

When the developer describes a meeting, Smriti identifies:

| Signal | Extraction |
|--------|-----------|
| "I agreed to / I said I would / I need to" | Your action item |
| "Priya/[name] will / said they would / is going to" | Dependency on that person |
| "by tomorrow / by EOD / by Friday / ASAP" | Deadline hint → informs reminder scheduling |
| "create a ticket / raise a Jira / open a PR" | Explicit Jira intent flagged on the item |
| "discuss / sync / align" | Softer commitment — Smriti asks "Is this something you need to follow up on?" |

---

*Section ends.*

---

## 9. Data Model

### Tables (SQLite)

```sql
-- Tasks
CREATE TABLE tasks (
  id            TEXT PRIMARY KEY,
  title         TEXT NOT NULL,
  description   TEXT,
  category      TEXT NOT NULL,         -- enum: 11 categories
  status        TEXT NOT NULL,         -- enum: Open|OnHold|InReview|Done|Abandoned
  priority      TEXT NOT NULL,         -- enum: Low|Medium|High|Critical
  parent_id     TEXT REFERENCES tasks(id),
  tags          TEXT,                  -- JSON array
  created_at    INTEGER NOT NULL,      -- unix timestamp
  updated_at    INTEGER NOT NULL,
  closed_at     INTEGER
);

-- Task logs (daily activity entries)
CREATE TABLE task_logs (
  id            TEXT PRIMARY KEY,
  task_id       TEXT NOT NULL REFERENCES tasks(id),
  content       TEXT NOT NULL,         -- what was done that day
  logged_at     INTEGER NOT NULL       -- unix timestamp
);

-- Task links (many-to-many, non-hierarchical relationships)
CREATE TABLE task_links (
  task_a        TEXT NOT NULL REFERENCES tasks(id),
  task_b        TEXT NOT NULL REFERENCES tasks(id),
  link_type     TEXT DEFAULT 'related',
  PRIMARY KEY (task_a, task_b)
);

-- Daily sessions (check-in log)
CREATE TABLE sessions (
  id            TEXT PRIMARY KEY,
  date          TEXT NOT NULL,         -- YYYY-MM-DD
  type          TEXT,                  -- checkin|recall|emotional|freeform
  tasks_logged  INTEGER DEFAULT 0,
  tasks_updated INTEGER DEFAULT 0,
  created_at    INTEGER NOT NULL
);

-- User settings
CREATE TABLE settings (
  key           TEXT PRIMARY KEY,
  value         TEXT NOT NULL
);
-- Keys: theme, notif_time, notif_enabled, user_name, streak_count, last_session_date
```

-- Action items (meeting commitments)
CREATE TABLE action_items (
  id              TEXT PRIMARY KEY,
  title           TEXT NOT NULL,
  meeting_context TEXT,
  owner           TEXT DEFAULT 'self',
  status          TEXT NOT NULL,    -- Pending|Snoozed|PickedUp|Promoted|Closed|Dropped
  promoted_task_id TEXT REFERENCES tasks(id),
  remind_at       INTEGER,
  reminder_count  INTEGER DEFAULT 0,
  notes           TEXT,
  created_at      INTEGER NOT NULL,
  updated_at      INTEGER NOT NULL,
  resolved_at     INTEGER
);

### Indexes
```sql
CREATE INDEX idx_tasks_status        ON tasks(status);
CREATE INDEX idx_tasks_category      ON tasks(category);
CREATE INDEX idx_tasks_created       ON tasks(created_at);
CREATE INDEX idx_logs_task           ON task_logs(task_id);
CREATE INDEX idx_logs_date           ON task_logs(logged_at);
CREATE INDEX idx_action_items_status ON action_items(status);
CREATE INDEX idx_action_items_remind ON action_items(remind_at);
CREATE INDEX idx_action_items_owner  ON action_items(owner);
```

---

## 10. Smriti — AI Assistant Design

### Model
- **Gemma 2B** (via `flutter_gemma` package) running fully on-device
- No internet required for AI features
- Fallback to rule-based parsing if model not available / device too old

### Intent Classification
Smriti classifies each user message into one of:

| Intent | Action |
|--------|--------|
| `task_log` | Extract task(s) from message, fuzzy match, confirm/create |
| `task_update` | Update status/details of an existing task |
| `action_item_capture` | Extract meeting action items with owners and deadlines |
| `action_item_followup` | Check status of pending action item |
| `dependency_track` | Track someone else's commitment as dependency |
| `emotional_vent` | Switch to appropriate emotional mode |
| `query_lookup` | Fuzzy search for a specific task |
| `query_aggregate` | SQL aggregate query + chart |
| `query_timeline` | Date-range filtered task list |
| `appraisal_export` | Generate summary for a period |
| `general_chat` | Respond conversationally without task action |

### Fuzzy Matching Algorithm
- Tokenise user input → extract candidate task phrases
- Score each against existing task titles using **Levenshtein distance + token overlap**
- Threshold: confidence > 0.65 → show confirmation card
- Confidence 0.4–0.65 → ask "Did you mean X?"
- Below 0.4 → treat as new task

### Prompt Design Principles
- System prompt includes: user's name, today's date, recent open tasks, category list
- Smriti's persona embedded in system prompt (warm, dev-aware, has opinions)
- Responses capped at 80 words for casual messages, 200 words for summaries
- Emotional mode prompts override default logging tone entirely

---

## 11. UI/UX Design

### Screens

| Screen | Purpose |
|--------|---------|
| **Smriti Chat** | Primary interaction screen — daily check-in, recall, emotional support |
| **My Work (Dashboard)** | Full task list with filters, stats, analytics |
| **Task Detail** | Timeline, logs, linked tasks, status management |
| **Insights** | Period-based charts, Smriti's narrative, export |
| **Settings** | Theme, notification time, name, data management |

### Navigation
Bottom navigation bar (4 items):
- 💬 Smriti (chat — default landing)
- 📋 My Work (dashboard)
- 📊 Insights
- ⚙️ Settings

### Design Tokens

| Token | Dark (Void Violet) | Light (Arctic Light) |
|-------|-------------------|---------------------|
| Background | `#0a0a0f` | `#f8f7ff` |
| Surface | `#1a1a2e` | `#ffffff` |
| Accent primary | `#8b5cf6` | `#6366f1` |
| Accent text | `#a78bfa` | `#818cf8` |
| Text primary | `#f0eeff` | `#1e1b4b` |
| Text secondary | `#a899d0` | `#4c4594` |
| Text muted | `#6b5f8a` | `#9ca3af` |
| User bubble | Violet gradient | Indigo gradient |
| Smriti bubble | Dark surface | White card |

### Typography
- **Display / headings:** Syne (800, 700)
- **Body / UI:** Instrument Sans (600, 500, 400)
- **Mono / labels / timestamps:** DM Mono (500, 400)

### Design Prototype
Interactive HTML prototype available at:
`/Users/keerthan.b/Desktop/worktrace-design/index.html`

Includes: Overview · Screens (6 phone mockups) · Themes · Smriti's Soul · User Flow · Tech Stack · Notification design · Full Spec

---

## 12. Technical Architecture

### Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Framework | Flutter (Dart) | Single codebase Android+iOS; excellent custom UI; hot reload |
| AI / NLP | `flutter_gemma` (Gemma 2B) | On-device, offline, free, private |
| Storage | SQLite via `sqflite` | Structured queries, fast, well-supported |
| State management | Riverpod | Clean, testable, reactive |
| Charts | `fl_chart` | Flexible, customisable Flutter charts |
| Notifications | `flutter_local_notifications` | Scheduled daily check-in, deep-link on tap |
| Calendar | `device_calendar` | Read holidays/leaves from device calendar |
| Export | `pdf` + `share_plus` | Generate PDF, share via system share sheet |
| Fuzzy match | Custom Dart (Levenshtein) | No external dependency needed |
| Preferences | `shared_preferences` | Theme, notif time, user name |

### Folder Structure
```
lib/
├── main.dart
├── app.dart                    # MaterialApp, theme, routing
├── core/
│   ├── theme/
│   │   ├── app_theme.dart      # ThemeData for dark + light
│   │   └── theme_provider.dart # Riverpod provider, SharedPrefs
│   ├── db/
│   │   ├── database.dart       # SQLite init, migrations
│   │   └── dao/
│   │       ├── task_dao.dart
│   │       ├── log_dao.dart
│   │       └── session_dao.dart
│   └── constants.dart
├── features/
│   ├── chat/
│   │   ├── chat_screen.dart
│   │   ├── chat_controller.dart
│   │   ├── widgets/
│   │   │   ├── message_bubble.dart
│   │   │   ├── task_confirm_card.dart
│   │   │   ├── quick_reply_chips.dart
│   │   │   └── inline_chart_bubble.dart
│   │   └── smriti/
│   │       ├── smriti_service.dart     # orchestrates Gemma + DB
│   │       ├── intent_classifier.dart  # classifies message intent
│   │       ├── task_extractor.dart     # extracts task mentions
│   │       ├── fuzzy_matcher.dart      # Levenshtein scoring
│   │       └── emotion_detector.dart   # detects emotional signals
│   ├── dashboard/
│   │   ├── dashboard_screen.dart
│   │   ├── dashboard_controller.dart
│   │   └── widgets/
│   │       ├── stat_card.dart
│   │       ├── category_bar.dart
│   │       ├── task_list_item.dart
│   │       ├── duration_heatmap.dart
│   │       └── open_tasks_chart.dart
│   ├── task_detail/
│   │   ├── task_detail_screen.dart
│   │   └── widgets/
│   │       ├── activity_log.dart
│   │       └── linked_tasks.dart
│   ├── insights/
│   │   ├── insights_screen.dart
│   │   ├── insights_controller.dart
│   │   └── widgets/
│   │       ├── velocity_chart.dart
│   │       ├── category_trend_chart.dart
│   │       ├── resolution_time_chart.dart
│   │       ├── focus_score_card.dart
│   │       ├── activity_heatmap.dart
│   │       └── smriti_narrative_card.dart
│   └── settings/
│       ├── settings_screen.dart
│       └── settings_controller.dart
├── models/
│   ├── task.dart
│   ├── task_log.dart
│   ├── task_link.dart
│   └── session.dart
└── services/
    ├── notification_service.dart
    ├── calendar_service.dart
    ├── export_service.dart         # PDF + share
    └── gemma_service.dart          # Gemma model wrapper
```

### Architecture Pattern
**MVVM + Repository:**
- `Screen` (View) ← `Controller` (ViewModel, Riverpod) ← `DAO` (Repository) ← SQLite
- `SmritiService` orchestrates: `IntentClassifier` → `FuzzyMatcher` → `DAO` → `GemmaService` → response

---

## 12. Phased Roadmap

### v1 — Foundation *(Build target: this sprint)*

**Scope:**
- Smriti chat UI (message bubbles, quick replies, task confirm card)
- Daily notification at configurable time
- Task CRUD: create, read, update status/logs
- SQLite local storage with full schema
- Rule-based intent parsing (Gemma integrated but not primary yet)
- Fuzzy duplicate detection (Levenshtein)
- Daily check-in flow end-to-end
- Open task nudge after today's tasks
- My Work dashboard: stats, category bars, task list, task detail
- Settings: theme, notification time, user name
- Dual theme (Void Violet dark / Arctic Light light), system-aware

**Not in v1:**
- Insights charts (basic period summary only)
- Emotional intelligence modes
- Parent-child task linking
- Calendar integration
- PDF export

---

### v2 — Intelligence *(~6 weeks after v1)*

**Scope:**
- Gemma on-device AI fully integrated for intent classification + response generation
- Emotion detection and all 7 Smriti response modes
- Parent-child task relationships
- Linked tasks (non-hierarchical)
- Device calendar integration (leave / holiday detection)
- Streak tracking and milestone notifications
- Pattern memory (recurring frustrations, uncategorised mentions)
- Search & Recall mode with inline charts
- Advanced fuzzy matching improvements

---

### v3 — Insights *(~8 weeks after v2)*

**Scope:**
- Full Insights screen: velocity chart, category trend, resolution time, cycle comparison, focus score, activity heatmap
- Smriti's written narrative (AI-generated)
- Weekly/monthly automated summary notification
- PDF export — formatted appraisal document
- Text export + share
- My Work analytics: duration heatmap, priority donut, task duration list

---

### v4 — Growth *(~12 weeks after v3)*

**Scope:**
- Optional encrypted cloud backup (Firebase or self-hosted)
- Jira / Linear import (read-only, to backfill historical tasks)
- Manager view (read-only shared summary, user controls what's shared)
- Team insights (org-level aggregates, opt-in)
- iOS release
- Google Play Store public release
- Career arc visualisation (multi-year view)

---

## 13. Build Approach

### Development Environment (Mac)

```bash
# 1. Install Flutter
brew install flutter

# 2. Install Android Studio
# → Download from developer.android.com/studio
# → Install Android SDK via SDK Manager
# → Create AVD (Android Virtual Device) emulator

# 3. Verify setup
flutter doctor

# 4. Create project
flutter create worktrace --org com.worktrace --platforms android

# 5. Run on emulator
flutter run

# 6. Hot reload (while running)
# Press 'r' in terminal

# 7. Release build (Play Store)
flutter build appbundle --release
```

### Key Dependencies (`pubspec.yaml`)

```yaml
dependencies:
  flutter_riverpod: ^2.5.1
  sqflite: ^2.3.2
  path: ^1.9.0
  flutter_local_notifications: ^17.2.1
  fl_chart: ^0.68.0
  flutter_gemma: ^0.2.0
  shared_preferences: ^2.2.3
  device_calendar: ^4.3.1
  pdf: ^3.10.8
  share_plus: ^9.0.0
  uuid: ^4.4.0
  intl: ^0.19.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  mocktail: ^1.0.3
  flutter_lints: ^3.0.0
```

   ---

## 13. Testing Strategy

### Unit Tests
- `FuzzyMatcher` — test Levenshtein scoring across edge cases
- `IntentClassifier` — test classification accuracy across message types
- `TaskDAO` — test all CRUD operations
- `SmritiService` — test full check-in flow with mock DAO

### Widget Tests
- Chat message bubbles (user / Smriti / task card / inline chart)
- Task confirm card (yes/no actions)
- Quick reply chips
- Dashboard stat cards

### Integration Tests
- Full daily check-in flow: notification → chat → task created → DB verified
- Duplicate detection: task mentioned twice → confirmation shown
- Theme switch: dark ↔ light ↔ system

### Manual QA Checklist
- [ ] Notification fires at scheduled time
- [ ] Deep-link from notification opens chat screen
- [ ] Fuzzy match works for misspelled task names
- [ ] Task created correctly for all 11 categories
- [ ] All 5 status transitions work
- [ ] Parent-child task link creates and displays correctly
- [ ] Dashboard stats match DB counts
- [ ] Theme persists across app restart
- [ ] App works fully offline (no crash on no network)

---

## 15. Publishing & Distribution

### Internal Distribution (v1)
- Build: `flutter build apk --release`
- Share APK directly via email / Drive / WhatsApp for internal testing
- No Play Store required for this stage

### Play Store (v4)
1. Create keystore: `keytool -genkey -v -keystore worktrace.jks ...`
2. Configure `build.gradle` with signing config
3. Build: `flutter build appbundle --release`
4. Google Play Console ($25 one-time developer fee)
5. Upload AAB → set up internal test track
6. Promote: internal → closed testing → open testing → production
7. First review: typically 1–3 business days

### App Store Metadata (when ready)
- **App name:** WorkTrace
- **Tagline:** Your career's memory. So you don't have to be.
- **Category:** Productivity
- **Target audience:** Software developers

---

## 17. Open Questions & Risks

### Open Questions

| Question | Owner | Priority |
|----------|-------|---------|
| Should the daily check-in time be per-day-of-week configurable? | Product | Low |
| What happens if Gemma model download fails? | Engineering | High |
| Should emotional conversations be logged/stored at all? | Product + Privacy | High |
| Multi-language support needed in v1? | Product | Low |
| Should "leave" be manually set or inferred from calendar only? | Product | Medium |
| Action items: max reminder count before escalating to "just drop it"? | Product | Medium |
| Dependency tracking: auto-resolve after N days without mention? | Product | Low |

### Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Gemma 2B too slow on older Android devices | Medium | High | Fallback to rule-based parsing; test on Android 8+ mid-range device |
| User retention drops after first week | Medium | High | Streak tracking, weekly nudges, quick wins visible early |
| Notification permission denied | High | High | Onboarding screen explaining value before permission request |
| Task data model too rigid for real use | Low | Medium | "Other" category + free tags give escape valve |
| Users find daily check-in annoying | Medium | High | Snooze option, adjustable time, skip with single tap |

---

*Document ends. For questions, contact the product team.*  
*Design prototype: `/Users/keerthan.b/Desktop/worktrace-design/index.html`*
