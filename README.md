# ascendops-agent-pack

A property management agent pack built on [cortextos](https://github.com/grandamenium/cortextos) — a framework for running persistent AI agents that manage operations around the clock.

This pack includes three agent roles, a full skill library, and a purpose-built property management skill set for work order triage, vendor coordination, and resident communication.

---

## Quick start (technical)

Requires [cortextos](https://github.com/grandamenium/cortextos) already installed and running.

```bash
# Clone this pack
git clone https://github.com/noogalabs/ascendops-agent-pack.git

# Copy an agent template into your org
cp -r ascendops-agent-pack/agents/specialist /path/to/your/cortextos/orgs/myorg/agents/myagent

# Copy skills you want to use
cp -r ascendops-agent-pack/skills/heartbeat /path/to/your/agent/.claude/skills/

# Edit IDENTITY.md and config.json for your agent, then start it
cortextos start myagent
```

See each agent's `CLAUDE.md` and `config.json` for configuration options.

---

## Getting started (step by step)

**What you need first:**
- A Mac or Linux machine running cortextos ([install guide](https://github.com/grandamenium/cortextos))
- A Telegram bot token (create one with [@BotFather](https://t.me/BotFather) — free, takes 2 minutes)
- About 30 minutes

**Step 1 — Clone this pack**

Open Terminal and run:
```bash
git clone https://github.com/noogalabs/ascendops-agent-pack.git
```

**Step 2 — Pick an agent role**

There are three roles included:
- `orchestrator` — coordinates other agents, delegates tasks, does not do specialist work itself
- `specialist` — does hands-on work: research, email drafts, data pulls, integrations
- `analyst` — focused on reporting, summaries, and insight generation

Most people start with one specialist agent.

**Step 3 — Copy the template**

```bash
# Replace myagent with whatever you want to call yours
cp -r ascendops-agent-pack/agents/specialist ~/.cortextos/orgs/default/agents/myagent
```

**Step 4 — Configure it**

Edit these files inside your new agent folder:
- `IDENTITY.md` — give your agent a name, role, and personality
- `config.json` — set timezone, session length, and which crons to run
- `.env` — add your Telegram bot token and chat ID

**Step 5 — Add skills**

Copy whichever skills from `skills/` match what you want the agent to do:
```bash
cp -r ascendops-agent-pack/skills/heartbeat ~/.cortextos/orgs/default/agents/myagent/.claude/skills/
cp -r ascendops-agent-pack/skills/tasks ~/.cortextos/orgs/default/agents/myagent/.claude/skills/
```

**Step 6 — Start it**

```bash
cortextos start myagent
```

Your agent will boot up and message you on Telegram.

---

## Property management agents

The `skills/pm/` directory contains a full skill set for property managers:

| Skill | What it does |
|-------|--------------|
| `pm-meld-triage` | Triages new work orders — checks photos, assesses urgency, recommends vendor |
| `pm-check-meld` | Pulls status on a specific work order |
| `pm-morning-scan` | Scans open work orders each morning and flags anything needing attention |
| `pm-dane-iq-playbook` | Decision playbook for common PM scenarios (vendor selection, escalation thresholds, etc.) |
| `pm-inspections` | Tracks upcoming and completed unit inspections |
| `pm-propertymeld-platform` | Reference for working with the PropertyMeld API |

These are designed to work alongside your existing property management software. They do not replace your PM platform — they sit on top of it and handle the triage, communication, and coordination layer.

---

## Hermes agent

The `agents/hermes/` template runs on the [Hermes](https://github.com/NousResearch/hermes-agent) Python REPL runtime instead of Claude Code. It participates in the bus identically — heartbeat, inbox, tasks, events — but uses SQLite for cross-session memory and manages its own scheduling.

**Two-step install:**

**Step 1 — Install the Hermes binary**

```bash
pip install hermes-agent
```

Verify:
```bash
which hermes
```

The daemon cannot start the agent until this binary is on PATH.

**Step 2 — Use this template**

```bash
cp -r ascendops-agent-pack/agents/hermes /path/to/cortextos/orgs/myorg/agents/myagent
```

Edit `config.json` (set `agent_name`, `timezone`) and `IDENTITY.md`, then:

```bash
cortextos start myagent
```

See `agents/hermes/CLAUDE.md` for full setup details and runtime differences vs Claude Code agents.

---

## What's included

See [AGENTS.md](AGENTS.md) for the full catalog of agents and skills.

---

## Contributing

Have a skill or agent role you have built that others might use? See [CONTRIBUTING.md](CONTRIBUTING.md).
