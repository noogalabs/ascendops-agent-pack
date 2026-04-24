---
name: pm-org-playbook-template
effort: low
description: "Org-specific PM playbook template. Fill in triage rules, vendor roster, escalation thresholds, and habitability standards for your portfolio."
triggers: ["pm playbook", "triage rules", "vendor selection", "escalation rules", "how urgent", "our playbook"]
---

# PM Operational Playbook — [YOUR ORG NAME]

> Replace every section below with your org's actual rules, vendors, and thresholds. This template shows the structure. Fill it in once and the agent uses it every time.

---

## Part 1: Triage Decision Tree

### Step 1 — Safety scan (run first, before anything else)

Define which keywords trigger each severity level. First match wins.

#### CRITICAL — evacuate / call immediately

| Trigger keywords | Action |
|-----------------|--------|
| [e.g. gas smell, sparking outlet, flooding] | [e.g. Evacuate. Call gas company. Do not use electrical switches.] |
| [add more] | [add action] |

#### HIGH — same-day response required

| Trigger keywords | Action |
|-----------------|--------|
| [e.g. no heat, sewage backup, no hot water] | [e.g. Emergency HVAC — health risk] |
| [add more] | [add action] |

**Exclusions:** List any keywords that override pattern matches.
Example: "gas station" or "gas grill" should NOT trigger a gas-leak alert.

#### MEDIUM — can wait for vendor, same-week response

[List issue types: leaks, power in one room, appliance problems, etc.]

#### LOW — routine, standard SLA

[List issue types: slow drain, light flickering, cosmetic issues, etc.]

---

### Step 2 — Life-safety temperature rules

| Condition | Severity |
|-----------|----------|
| No heat + outdoor temp at or below [X°F] | CRITICAL |
| No heat + vulnerable occupants (elderly, infants, medical) | CRITICAL at any temp |
| No AC + outdoor temp at or above [X°F] + vulnerable occupants | CRITICAL |
| [add your thresholds] | |

---

### Step 3 — Habitability rules

List conditions that make a unit legally uninhabitable in your jurisdiction. When these are present, escalate immediately regardless of other priority.

- [ ] No heat in winter (define threshold: below ___°F outdoor temp)
- [ ] Sewage backup / flooding inside the unit
- [ ] No running water
- [ ] Structural damage (ceiling collapse, wall failure)
- [ ] [Add state/local requirements]

---

## Part 2: Vendor Selection

### In-house vs. outside vendor threshold

**Default rule:** [e.g. "Use in-house tech first for any trade we cover. If no availability within 48 hours, escalate to outside vendor."]

### In-house technicians

| Name | Trades | Contact | Notes |
|------|--------|---------|-------|
| [Name] | [e.g. HVAC, electrical, plumbing] | [phone] | [e.g. on-call Mon-Fri] |
| [Name] | | | |

### Outside vendor roster

| Trade | Vendor name | Contact | When to use |
|-------|-------------|---------|-------------|
| Plumbing | [Company] | [phone] | Emergencies, overflow |
| HVAC | [Company] | [phone] | Emergencies, in-house unavailable |
| Electrical | [Company] | [phone] | Permit work, panel upgrades |
| Pest control | [Company] | [phone] | All pest issues |
| [Add more] | | | |

### Vendor scoring (optional)

If you rank vendors, define your criteria:
- Response time
- Price
- Quality of work
- Reliability

---

## Part 3: Escalation Thresholds

### When to notify the property manager

- Any CRITICAL issue
- Any HIGH issue with no same-day vendor availability
- Repeat issues at the same unit (define: [X] or more in [Y] days)
- Costs expected to exceed $[amount]
- Resident requests manager contact directly

### When to notify the property owner

- Costs expected to exceed $[amount]
- Habitability issues
- Legal notices received
- [Add your thresholds]

### When to call 911 or emergency services (not a vendor)

- Active fire
- Gas leak with evacuation required
- Structural collapse
- [Add any others]

---

## Part 4: Communication Standards

### Resident acknowledgment SLA

- CRITICAL: Acknowledge within [X] minutes
- HIGH: Acknowledge within [X] hours
- MEDIUM: Acknowledge within [X] business hours
- LOW: Acknowledge within [X] business days

### Standard acknowledgment message

> [Write the message you want the agent to send residents when a work order is received. Example:]
> "Hi [name], we received your work order for [issue]. We will have someone in touch within [timeframe]. Thank you for letting us know."

### Vendor scheduling message

> [Write the message sent to residents when a vendor is scheduled. Example:]
> "Hi [name], we have scheduled [vendor/tech] for [date/time window]. They will contact you [X] hour(s) before arriving."

---

## Part 5: Property and Unit Reference

### Portfolio overview

| Property | Units | Key notes |
|----------|-------|-----------|
| [Address] | [count] | [e.g. older wiring, no A/C, well water] |
| [Address] | | |

### Units with known issues or special handling

| Unit | Issue | Notes |
|------|-------|-------|
| [Unit] | [e.g. recurring plumbing] | [what to watch for] |
| [Unit] | | |

---

## How to use this playbook

When a new work order comes in, the agent runs through the steps in order:

1. Safety scan (Part 1, Step 1) — if it matches CRITICAL, act immediately
2. Life-safety and habitability check (Steps 2-3)
3. Vendor selection (Part 2) — in-house first, outside if unavailable
4. Escalation check (Part 3) — notify manager/owner if thresholds hit
5. Send resident acknowledgment (Part 4)

Fill in every section with your actual data. The more specific you are, the more consistent the agent's decisions will be.
