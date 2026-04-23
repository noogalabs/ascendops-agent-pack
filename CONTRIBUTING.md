# Contributing

If you have built an agent or skill that works well in a property management context, we would like to include it here.

## What belongs in this pack

- Agent role templates that are reusable across different PM operations
- Skills that other property managers could drop in and use with minimal configuration
- PM platform integrations (work order systems, tenant portals, accounting tools)
- Communication skills (vendor follow-up, resident messaging, owner reporting)

What does not belong here: company-specific playbooks, credentials, private data, or anything hardcoded to a single org.

---

## How to submit

1. Fork this repo
2. Add your agent or skill following the structure below
3. Open a pull request with a brief description of what it does and what you tested it on

We review PRs on a rolling basis. If you are not sure whether something fits, open an issue first.

---

## Submission template

When opening a pull request, include the following in your PR description:

```
### Name
[Short name for the agent or skill]

### Type
[ ] Agent template
[ ] Skill

### Description
What does this do? What problem does it solve for a property manager?

### Integrations required
List any external tools, APIs, or credentials this needs to work.
Example: PropertyMeld API key, Twilio account, Google Calendar access

### Install steps
1. Copy [files] to [location]
2. Set [env vars or config values]
3. Add to CLAUDE.md or config.json as follows: ...

### Tested on
What setup did you test this on? (OS, cortextos version, which PM platform)

### Notes
Anything else reviewers should know — known limitations, future work, dependencies.
```

---

## File structure

**Agent template:**
```
agents/
  your-role-name/
    IDENTITY.md       # Who the agent is
    SOUL.md           # Values and working style
    CLAUDE.md         # Session instructions
    GUARDRAILS.md     # What it must never do
    HEARTBEAT.md      # Heartbeat instructions
    GOALS.md          # Standing objectives
    MEMORY.md         # Memory index (starts empty)
    TOOLS.md          # Available tools reference
    USER.md           # Profile of the human it works with
    config.json       # Crons, session settings
```

**Skill:**
```
skills/
  your-skill-name/
    SKILL.md          # The skill instructions the agent reads and follows
    README.md         # Optional: human-readable explanation
```

Keep skill instructions in `SKILL.md` at the root of the skill directory. The agent reads this file when the skill is invoked.

---

## Style notes

- Write skill instructions as direct commands to the agent, not documentation for a human
- Avoid hardcoding org names, property addresses, or vendor contacts
- Use `$CTX_AGENT_NAME`, `$CTX_ORG`, and other cortextos env vars where you need to reference the agent or org
- Test that your skill works on a fresh agent with no prior context
