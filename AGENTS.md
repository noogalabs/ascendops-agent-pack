# Agent and Skill Catalog

## Agent Templates

### orchestrator
Coordinates other agents. Receives tasks from humans, delegates to specialists, tracks progress, and reports back. Does not do specialist work itself — that is by design.

**Best for:** Teams with multiple agents where someone needs to manage the queue and route tasks.

**Key files:** `IDENTITY.md`, `SOUL.md`, `CLAUDE.md`, `config.json`

---

### specialist
General-purpose hands-on agent. Does research, drafts communications, pulls data, runs integrations, manages files, and executes tasks directly.

**Best for:** A single do-it-all agent, or a role-specific worker (e.g. a comms specialist, a data specialist).

**Key files:** `IDENTITY.md`, `SOUL.md`, `CLAUDE.md`, `config.json`

---

### analyst
Focused on reporting, summaries, and insight generation. Reads data, builds reports, and surfaces patterns. Less action-oriented than a specialist.

**Best for:** Regular reporting agents — morning briefings, weekly reviews, metric digests.

**Key files:** `IDENTITY.md`, `SOUL.md`, `CLAUDE.md`, `config.json`

---

## Core Skills

### heartbeat
Keeps the agent's presence alive in the fleet. Updates a heartbeat file on a schedule, checks inbox, and logs activity. Required for dashboard visibility.

**Trigger:** Cron (typically every 2-4 hours)

---

### tasks
Full task lifecycle management — create, start, complete, and log tasks so they show up in the dashboard and contribute to effectiveness scoring.

**Trigger:** Called during any significant work

---

### approvals
Approval workflow for actions that require human sign-off before proceeding. Sends approval requests via Telegram and waits for a yes/no.

**Trigger:** Any action tagged as requiring approval

---

### knowledge-base
Query and ingest documents into the org knowledge base using natural language. Lets agents find and store institutional knowledge.

**Trigger:** Called when the agent needs to look something up or store new knowledge

---

### cron-management
Manages recurring cron jobs — add, remove, and verify crons from `config.json`. Prevents duplicates on restart.

**Trigger:** Session start, or when cron configuration changes

---

### morning-review
Morning briefing skill — surfaces open tasks, upcoming events, and anything flagged overnight.

**Trigger:** Cron (typically 7:00-8:00 AM)

---

### evening-review
End-of-day wrap-up — summarizes what was completed, flags unresolved items, and sets context for the next session.

**Trigger:** Cron (typically 6:00-7:00 PM)

---

### nighttime-mode
Reduces agent activity during off-hours. Suppresses non-urgent notifications and defers non-critical tasks until morning.

**Trigger:** Cron (transitions to night mode in the evening)

---

### weekly-review
Weekly summary of completed work, open items, and patterns. Good for accountability and planning.

**Trigger:** Cron (typically Sunday evening or Monday morning)

---

### goal-management
Tracks and manages long-term goals. Lets the agent maintain a running list of objectives and check progress over time.

**Trigger:** Periodic review, or when goals are updated

---

## Property Management Skills (`skills/pm/`)

### pm-meld-triage
Core triage workflow for incoming work orders. Checks photos, assesses urgency, evaluates against in-house vs vendor thresholds, and prepares a recommendation for the property manager.

**Requires:** PropertyMeld access, vendor contact list

---

### pm-check-meld
Pulls current status on a specific work order by ID or description.

**Requires:** PropertyMeld access

---

### pm-morning-scan
Daily scan of open work orders. Flags items over the age threshold, items missing photos or notes, and anything with status changes overnight.

**Trigger:** Cron (morning, before the manager's day starts)

---

### pm-dane-iq-playbook
Decision playbook for common property management scenarios: vendor selection by trade, escalation thresholds, emergency vs non-emergency classification, resident communication standards.

**Best for:** Ensuring consistent decisions across agents and sessions

---

### pm-inspections
Tracks upcoming unit inspections, logs completion status, and flags units that are overdue.

**Requires:** Inspection schedule data

---

### pm-propertymeld-platform
Reference skill for the PropertyMeld API — endpoints, authentication, data structures, and common queries. Used by other PM skills as a foundation.
