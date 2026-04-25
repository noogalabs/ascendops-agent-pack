---
name: pm-cli-harness
effort: low
description: "How to install and operate snapcli-pm for Property Meld. Covers tool hierarchy, install, and non-obvious operational rules. Run 'pm --help' or 'pm work-orders --help' for the live command list."
triggers: ["pm work-orders", "pm cli", "snapcli pm", "pm command", "work order cli", "assign tech cli", "schedule appointment cli", "pm merge", "pm complete", "pm cancel", "pm schedule", "pm tenants"]
---

# Property Meld CLI (snapcli-pm)

## Install

```bash
pip install "git+https://github.com/noogalabs/snapcli.git#subdirectory=adapters/pm"
```

For all available commands:
```bash
pm --help
pm work-orders --help
pm tenants --help
```

Required env / credentials:
```
PM_CREDS_PATH=~/.snapcli/property-meld.json   # session cookies (see pm-session-recapture skill)
PM_CLIENT_ID=...                               # Nexus API — reads only
PM_CLIENT_SECRET=...
```

---

## Tool Hierarchy — Always Follow This Order

1. **`pm` (snapcli)** — primary for ALL operations. Plain HTTP, no browser. Use this first.
2. **Nexus API** (OAuth2, `PM_CLIENT_ID/SECRET`) — secondary. Good for bulk reads and `maintenance_notes`. Cannot write assignments, schedules, or chat.
3. **PM browser UI** — only if snapcli is broken (expired cookies). Do not fall back here just because Nexus returns 404.

---

## Non-Obvious Rules (not in --help)

**schedule**
- `--dtstart` must include timezone: `2026-04-27T14:00:00-04:00` not `2026-04-27T14:00:00`
- In-house tech must be assigned first — PM creates the appointment object at assignment time
- `--hours` defaults to 2.0, matching the PM UI default

**merge**
- Both melds must be at the same property unit or the API rejects with "Destination Meld not found"
- Source meld gets `MANAGER_CANCELED` status with "(Merged)" prefix appended to title

**complete**
- Meld must be in `PENDING_COMPLETION` status or the request returns 403
- Applies to any in-house tech (not vendor-specific)

**cancel**
- `--reason` sets `manager_cancellation_reason` — include it for audit trail

**tenants list --search**
- Server has no filter params — search is client-side across name, email, and phone
- Fetches up to 200 records per page before filtering

**health check**
```bash
pm probe --json   # returns {"ok": true} if session is valid
```
