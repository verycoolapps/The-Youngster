# Temporal Reasoning Reference
## THE YOUNGSTER — Eternal Memory & Continuity Intelligence

---

## The Time Problem

Most AI systems exist in an eternal present. They have no concept of:
- What was true yesterday but changed today
- What was planned for next month
- What was promised but not yet delivered
- Why a decision made three weeks ago is still relevant now

THE YOUNGSTER thinks across time. It maintains a coherent timeline of past, present, and future — and distinguishes between them with precision.

---

## Temporal Classification System

### Past Event Types

| Type | Description | Example |
|---|---|---|
| `completed_task` | A task confirmed as done | "Homepage redesign was delivered Nov 5" |
| `historical_decision` | A decision made in the past | "We chose React over Vue on Oct 15" |
| `completed_milestone` | A project milestone achieved | "Phase 1 launched successfully" |
| `resolved_blocker` | A blocker that was removed | "API access issue resolved by backend team" |
| `past_conversation` | An important previous discussion | "Discussed pricing model last session" |
| `previous_version` | An earlier version of a plan or spec | "Original deadline was Dec 10 before it moved" |

### Present State Types

| Type | Description |
|---|---|
| `active_task` | Currently being worked on |
| `active_project` | Ongoing project in progress |
| `pending_decision` | Decision that needs to be made now |
| `active_blocker` | Something currently stopping progress |
| `current_priority` | What matters most right now |
| `in_review` | Work submitted and awaiting feedback |

### Future Event Types

| Type | Description | Confidence |
|---|---|---|
| `confirmed_deadline` | Hard deadline formally agreed upon | High |
| `planned_milestone` | Milestone in the project plan | Medium-High |
| `proposed_date` | Date mentioned but not yet confirmed | Medium |
| `intention` | Casual forward-looking statement | Low |
| `dependency` | Something that must happen before next step | Variable |

---

## Temporal Language Recognition

THE YOUNGSTER recognizes temporal signals in natural language:

### Past Signals
```
"was", "were", "had", "did", "finished", "completed", "decided",
"previously", "last time", "before", "earlier", "already", "used to"
```

### Present Signals
```
"is", "are", "currently", "now", "today", "at the moment",
"working on", "in progress", "right now", "this week"
```

### Future Signals
```
"will", "going to", "planning to", "intend to", "next", "upcoming",
"tomorrow", "next week", "next month", "by [date]", "before [date]"
```

### Uncertainty Signals
```
"maybe", "perhaps", "might", "possibly", "probably", "thinking about",
"considering", "if", "hoping", "aiming for"
```
→ These produce `intention` or `proposal` records, NOT `confirmed` ones.

### Commitment Signals
```
"will", "promise", "commit to", "guarantee", "definitely", "absolutely",
"confirmed", "agreed", "scheduled for", "it's set for"
```
→ These produce `confirmed` commitment records.

---

## Relative Date Handling

Always convert relative dates to absolute dates when recording:

| Statement | Recorded As |
|---|---|
| "by Friday" | Actual date of the upcoming Friday |
| "next month" | First day of the upcoming month (tentative range) |
| "in two weeks" | Exact date calculated from current date |
| "Q1 2027" | 2027-01-01 to 2027-03-31 |
| "end of year" | 2026-12-31 |
| "asap" | Priority: critical, no specific date |
| "eventually" | No date, intention only |

**Rule:** Always record when the date was relative to, so context is preserved.

**Example:**
```json
{
  "due_date": "2026-11-15",
  "due_date_original_statement": "by next Friday",
  "due_date_calculated_from": "2026-11-08"
}
```

---

## Timeline Conflict Resolution

### Scenario: Deadline Changed

**Input:**
- Record A: "Deadline is December 10" (timestamp: Oct 1)
- Record B: "We moved the deadline to December 20" (timestamp: Nov 8)

**Resolution:**
1. Record B supersedes Record A for CURRENT deadline
2. Record A is preserved in history
3. Current deadline = December 20 (confidence: verified if user stated explicitly)
4. Historical record shows change occurred Nov 8

**Output representation:**
```json
{
  "current_due_date": "2026-12-20",
  "original_due_date": "2026-12-10",
  "deadline_changed_at": "2026-11-08",
  "history": [
    {"due_date": "2026-12-10", "set_at": "2026-10-01"},
    {"due_date": "2026-12-20", "set_at": "2026-11-08"}
  ]
}
```

---

### Scenario: Conflicting Information Without Clear Supersession

**Input:**
- Record A: "The deadline is December 10" (timestamp: Nov 5)
- Record B: "The deadline is December 15" (timestamp: Nov 7)

No explicit statement that one supersedes the other.

**Resolution:**
1. Flag conflict explicitly
2. Present both records with timestamps
3. Note that Nov 7 is more recent (slight preference)
4. Ask ONE focused clarification: "I have two different deadlines on record — December 10 from Nov 5, and December 15 from Nov 7. Which is current?"
5. Do NOT assume one is correct

---

## Temporal Awareness in Responses

### When Retrieving Past Information

Always include temporal context:
- When did this happen?
- Was this ever updated or superseded?
- How long ago was this?
- Is this still relevant?

**Example:**
> "The decision to use PostgreSQL was made on October 15 — about three weeks ago. This has not been changed since."

### When Retrieving Present Information

Always verify currency:
- Is this the latest known state?
- Has this been updated since it was last retrieved?
- Are there any pending updates?

**Example:**
> "As of the last update (Nov 7), the homepage redesign is in review. I don't have confirmation that this has changed."

### When Retrieving Future Information

Always label confidence:
- Is this confirmed or proposed?
- How was this date set?
- Are there dependencies that could affect it?

**Example:**
> "The launch is currently planned for December 20. This is proposed — I don't have an explicit confirmation that it's locked in."

---

## Temporal Reasoning Heuristics

### Recency Preference
More recent information generally supersedes older information — unless the older information is a confirmed commitment and the newer is casual.

### Confidence Hierarchy
```
Explicit confirmation > Verified action > Direct statement > Inference > Assumption
```

### Preservation Rule
No temporal record is deleted due to being superseded. All historical states are preserved in `history[]`.

### Void vs. Unknown
- **Void:** "The deadline was removed" (explicitly cleared)
- **Unknown:** "I don't have a deadline on record for this task"

These are different. Never confuse them.

---

## The Time-Awareness Report

When asked "What changed since last time?":

```markdown
# CHANGE REPORT
*Comparing: Last session → Current state*

## STATUS CHANGES
| Item | Was | Now | Changed At |
|---|---|---|---|
| Homepage redesign | IN_PROGRESS | IN_REVIEW | 2026-11-07 |
| API deadline | Dec 10 | Dec 20 | 2026-11-08 |

## NEW ITEMS
- task_088: Mobile push notifications — Added Nov 8
- proj_005: Analytics dashboard — New project initiated Nov 7

## COMPLETED ITEMS
- task_033: Database schema v2 — Completed Nov 6
- task_034: User authentication — Completed Nov 7

## NO CHANGE
- Mobile app overall — Still ACTIVE, Phase: Development
- Deployment decision — Still pending

## UNKNOWN CHANGES
- Items not discussed in last session may have changed externally.
  These cannot be detected without user update.
```

---

*Reference document for THE YOUNGSTER v1.0 — Temporal Reasoning*
