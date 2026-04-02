---
name: lead-automation
description: >
  Lead capture + auto-reply + follow-up automation for Nevelto micro-agency. Trigger this skill whenever the user mentions leads, WhatsApp auto-reply, saving contacts, follow-up reminders, lead tracking, or any variation of "someone messaged → save + reply". Activates for both design and implementation requests. Stack: Baileys + Firebase + node-cron. No n8n. Output is always Trigger → Flow → Code-ready structure.
---

# Lead Automation Skill

Think in flows. Output code-ready structure only.

---

## Core Flow

```
Trigger: WhatsApp message received
→ Validate: extract name, phone
→ Check: duplicate in Firestore
→ If new → save doc, tag "new_lead"
→ Send: auto-reply via Baileys
→ Schedule: cron flag for +24h reminder
Output: Lead saved, replied, queued
```

---

## Output Format (MANDATORY)

```
GOAL: (one line)

FLOW:
Trigger: [event]
→ [step]
→ [step]
Output: [result]

COMPONENTS:
- [tool] — [why]

EDGE CASES:
1. [case] → [handler]
2. [case] → [handler]
3. [case] → [handler]

OPTIMIZATION:
- [reduction]
```

---

## Stack

| Layer | Tool |
|-------|------|
| Messaging | Baileys |
| Storage | Firestore |
| Scheduling | node-cron |
| Runtime | Node.js (single file) |

---

## Firestore Schema

```js
leads/{phone} = {
  name: string,
  phone: string,
  source: "whatsapp" | "form",
  status: "new_lead" | "followed_up" | "closed",
  createdAt: timestamp,
  updatedAt: timestamp
}
```

---

## Reusable Patterns

### Lead Capture
```
Trigger: msg received
→ Extract name + phone
→ Firestore: doc exists? → update updatedAt only
→ If new → set doc + tag "new_lead"
→ Baileys: send auto-reply
→ Set cron flag
Output: saved + replied
```

### Follow-Up Reminder
```
Trigger: node-cron (every hour)
→ Query: status="new_lead" AND createdAt > 24h ago
→ Send WhatsApp follow-up per lead
→ Update: status = "followed_up"
Output: reminders sent, statuses updated
```

### Daily Report
```
Trigger: cron "0 9 * * *"
→ Query: last 24h leads
→ Aggregate by status
→ Format 5-line summary
→ Send to owner WhatsApp
Output: daily snapshot
```

---

## Edge Case Defaults

1. Duplicate phone → `updatedAt` update only, skip insert
2. Baileys session drop → try/catch, log, don't crash
3. Double reminder → gate on `status` flag before sending
4. Firestore write fail → retry once, then console.error + alert owner
5. Empty input → validate before processing, send fallback reply

---

## Behavior Rules

- Max 120 words per response
- Arrows and bullets only
- No filler, no examples unless asked
- If unclear → ask 1 question only
- Never suggest n8n for <5 integrations
- Single Firestore write per lead (all fields at once)
- Hardcode reply templates (faster than dynamic)
- Cron hourly — never per-event for reminders

---

## Anti-Patterns

- ❌ n8n for 3-step flows
- ❌ Google Sheets for lead storage
- ❌ AI bot before keyword matching works
- ❌ Multiple Firestore writes per lead
- ❌ Per-event cron (use batch hourly)

---

## Escalation Path

```
Level 1: Single Node.js file (now)
Level 2: PM2 + per-client sessions
Level 3: VPS + n8n orchestration
Level 4: BullMQ + microservices
```

Don't jump levels.