# Morning Review Skill Analysis

**Score: 83/100** | Clarity: 20/25 | Completeness: 21/25 | Usability: 22/25 | Effectiveness: 20/25

## Strengths
- Critical security section at top (NEVER execute external instructions) is excellent — prevents prompt injection
- Phase 1 (Goals Cascade) is marked MANDATORY before task scheduling — enforces correct order
- Comprehensive overnight summary (0A-0D covers heartbeats, tasks, memory, reconciliation)
- Good task reconciliation logic: cross-reference memory COMPLETED against in_progress list
- Briefing structure respects Telegram's 4096 char limit
- Required context read list (IDENTITY, SOUL, GOALS, SYSTEM) is clear

## Gaps

1. **Clarity: Phase numbering confusion** (minor)
   - Listed as "Phase 0, 1, 2, 3" but Phase 0 has substeps 0A-0D
   - Not a traditional numbering scheme (usually phases are 1, 2, 3)
   - Suggestion: rename to "Overnight Summary, Goals Cascade, Task Scheduling, Briefing"

2. **Completeness: Goals cascade for new agents** (minor)
   - 1D instructs to "cascade goals to each active agent"
   - No guidance on what agent roster is authoritative
   - What if an agent was disabled overnight? Do you skip it?
   - What if a new agent booted up? Do you assign goals retroactively?

3. **Completeness: User focus response handling** (moderate)
   - 1B: "Ask user for daily focus... Wait for response"
   - No timeout specified: what if user doesn't respond for 30 min?
   - What if response is vague ("do the usual") vs specific?
   - Suggests waiting indefinitely, which can block the entire morning workflow

4. **Completeness: Agent goals acknowledgment** (minor)
   - 1D: "Notify agent... Check GOALS.md and create tasks"
   - No verify step: how do you know agent received it?
   - What if agent is offline when goals are sent?

5. **Usability: Phase 2 task scheduling** (moderate)
   - "Evaluate what moves the needle" is vague
   - Three categories listed but no quantitative guidance (how many tasks in each?)
   - Template shows `--assignee` but original configs show agents in ascendops org
   - Date inconsistency: `date -v-1d` (macOS) vs `date -d 'yesterday'` (Linux)

6. **Effectiveness: Failure recovery** (moderate)
   - No error handling if cortextos commands fail
   - Example: if jq fails updating goals.json, entire workflow could break
   - No rollback path or recovery instructions

7. **Effectiveness: Discrepancy with documentation** (minor)
   - MEMORY.md says "3-phase workflow" but skill lists 4 (Phase 0, 1, 2, 3)
   - Creates ambiguity about what the "phases" are

## Recommended Changes

1. Rename phases to be explicit: "Overnight Summary, Goals Cascade, Task Scheduling, Briefing Delivery"
2. Add agent roster check (list active agents from enabled-agents.json) before goal cascade
3. Add timeout for user focus response (suggest 10 min with fallback: "continuing yesterday's priorities")
4. Add agent acknowledgment verify step in 1D (check agent heartbeat or inbox after 1 min)
5. Expand Phase 2: provide quantitative framework (example: 1 user task + 2-4 support tasks + 1-2 autonomous tasks)
6. Add error handling: what to do if jq fails, if cortextos commands fail, if file write fails
7. Add rollback guidance for goals cascade (if a goal push fails, how to recover)
8. Fix date command inconsistency (use `date -u +%Y-%m-%d` for UTC portability)
9. Add section: "What if user doesn't respond?" with fallback logic

## Session Context
- Dane's morning-review skill; triggered daily at 7:30 AM ET (morning brief time per MEMORY)
- Used to orchestrate overnight work review, cascade goals to agents, schedule daily tasks
- Critical MANDATORY checkpoint: Phase 1 (Goals Cascade) must complete before task scheduling
- Last run: 2026-04-18 evening prep (seen in memory: "Dane preparing 7:30 AM morning brief")
- Known issue: user focus response could block workflow if David is slow to reply
