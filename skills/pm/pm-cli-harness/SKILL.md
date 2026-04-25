---
name: pm-cli-harness
effort: low
description: "Full CLI reference for snapcli-pm — all Property Meld read and write operations via plain HTTP. Covers work orders (list, get, comments, message, clone, merge, complete, cancel, schedule), tenants, properties, vendors, tech assignment, and API key rotation. Use this before reaching for the browser or Nexus API."
triggers: ["pm work-orders", "pm cli", "snapcli pm", "pm command", "work order cli", "assign tech cli", "schedule appointment cli", "pm merge", "pm complete", "pm cancel", "pm schedule", "pm tenants"]
---

# Property Meld CLI Harness (snapcli-pm)

## Tool Hierarchy

1. **snapcli (`pm`)** — primary tool for ALL PM operations. Plain HTTP with captured session cookies. Use this first.
2. **Nexus API** (`PM_CLIENT_ID`/`PM_CLIENT_SECRET`) — secondary. Good for bulk reads and `maintenance_notes` writes. Cannot write assignments, schedules, or chat.
3. **Manual PM UI** — last resort only if snapcli is broken (expired cookies, site layout change).

## Install

```bash
pip install git+https://github.com/noogalabs/snapcli.git#subdirectory=pm
# or from local clone:
pip install -e ~/projects/snapcli/pm/
```

Required env vars:
```
PM_CREDS_PATH=~/.snapcli/property-meld.json   # captured session cookies
PM_CLIENT_ID=...                               # Nexus API (reads only)
PM_CLIENT_SECRET=...
```

Session cookies are captured once via `pm-recapture-session.py` and stored in `PM_CREDS_PATH`. They last weeks to months. When they expire, run the recapture script (see pm-session-recapture skill).

## Health Check

```bash
pm probe --json   # verify cookies are valid and API is reachable
```

---

## Work Order Commands

```bash
# List
pm work-orders list --status open --json          # open work orders
pm work-orders list --status pending --json       # pending completion
pm work-orders list --limit 50 --json             # more results

# Read
pm work-orders get <meld_id> --json               # full work order detail
pm work-orders comments <meld_id> --json          # full message thread

# Write
pm work-orders send-message --meld-id <id> --text "message" --json
pm work-orders send-message --meld-id <id> --text "msg" --hide-tenant --json

pm work-orders clone --meld-id <id> --json
pm work-orders clone --meld-id <id> --description "Override title" --json

pm work-orders merge --meld-id <src> --into <dst> --json   # both must be at same unit
pm work-orders complete --meld-id <id> --json              # meld must be PENDING_COMPLETION
pm work-orders complete --meld-id <id> --notes "text" --json
pm work-orders cancel --meld-id <id> --json
pm work-orders cancel --meld-id <id> --reason "text" --json

# Scheduling (in-house tech must be assigned first)
pm work-orders schedule --meld-id <id> --dtstart 2026-04-27T14:00:00-04:00 --json
pm work-orders schedule --meld-id <id> --dtstart 2026-04-27T14:00:00-04:00 --hours 4 --json
```

### Scheduling Notes
- `--dtstart` must be ISO 8601 with timezone, e.g. `2026-04-27T14:00:00-04:00` for 2pm ET
- `--hours` defaults to 2.0 — matches the "2 hours" default in the PM UI
- The meld must already have an in-house tech assigned (PM creates the appointment object at assignment time)
- Works for any in-house tech (not vendor-specific)
- Merge requires both melds to be at the same property unit

---

## Tech & Vendor Assignment

```bash
# In-house tech (CLI command)
pm assign-tech --work-order-id <meld_id> --tech Carlos --json
pm assign-tech --work-order-id <meld_id> --tech Casey --json

# External vendor (http_backend directly — no dedicated CLI command yet)
python3 -c "
import sys; sys.path.insert(0, '~/projects/snapcli/pm')
from cli_anything.propertymeld import http_backend
creds = http_backend._load_creds()
cookie_hdr = http_backend._cookie_header(creds)
csrf = http_backend._get_csrf_token(cookie_hdr)
vendor_obj = {'id': <vendor_id>, 'type': 'Vendor', 'composite_id': '1-<vendor_id>'}
http_backend._http_patch('melds/<meld_id>/assign-maintenance/', {'maintenance': [vendor_obj]}, cookie_hdr, csrf)
"
```

---

## Tenant Commands

```bash
pm tenants list --json                            # all tenants (up to 100)
pm tenants list --search "Christy" --json         # filter by name, email, or phone substring
pm tenants list --search "(423) 400" --json       # search by phone
pm tenants get <tenant_id> --json                 # full tenant detail with contact info
```

Note: tenant search is client-side (server has no filter params). Returns first 100 matches.

---

## Properties & Vendors

```bash
pm properties list --json                         # all properties (Nexus API)
pm vendors list --json                            # all vendors (Nexus API)
```

---

## API Key Management

```bash
pm api-keys list --json                           # existing Nexus partner API keys
pm api-keys rotate --json                         # create new key (secret shown once)
pm api-keys rotate --update-railway --json        # rotate + push to Railway env vars
```

---

## Backend Reference

| Command | Endpoint | Auth |
|---------|----------|------|
| work-orders list/get/comments | Nexus API `/meld/` | PM_CLIENT_ID/SECRET |
| work-orders send-message | `POST comments/` | PM session cookies |
| work-orders clone | `POST melds/{id}/clone/` | PM session cookies |
| work-orders merge | `POST melds/{id}/merge/` | PM session cookies |
| work-orders complete | `PATCH melds/{id}/complete/` | PM session cookies |
| work-orders cancel | `PATCH melds/{id}/cancel/` | PM session cookies |
| work-orders schedule | `PUT management-appointments/{appt_id}/schedule/` | PM session cookies |
| assign-tech | `PATCH melds/{id}/assign-maintenance/` | PM session cookies |
| tenants list/get | `GET tenants/` or `tenants/{id}/` | PM session cookies |
| properties/vendors list | Nexus API | PM_CLIENT_ID/SECRET |

---

## Known Gaps

- Appointment scheduling for vendors — vendors set their own windows; no PM-side write endpoint found
- Chat message deletion/editing — browser UI only
- Tenant create/update — endpoint exists (`PUT tenants/{id}/`) but not wired as a CLI command yet
