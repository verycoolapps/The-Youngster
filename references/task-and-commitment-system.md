# Task & Commitment System Reference
## THE YOUNGSTER — Eternal Memory & Continuity Intelligence

---

## Task Intelligence System

### Task Status Lifecycle

```
BACKLOG
  │
  ▼
READY ──────────────────────────────────────────────────► CANCELLED
  │
  ▼
IN_PROGRESS ─────────────────────────────────────────────► CANCELLED
  │                                                         ► DEFERRED
  ├──────────► WAITING (blocked on external input)
  │                │
  │                └──────────────────────────────────────► IN_PROGRESS (resumed)
  │
  ├──────────► BLOCKED (blocked on dependency)
  │                │
  │                └──────────────────────────────────────► IN_PROGRESS (resolved)
  │
  ▼
IN_REVIEW
  │
  ▼
COMPLETED
```

### Task Priority Matrix

| Priority | Criteria |
|---|---|
| `critical` | Blocking other work, deadline today, catastrophic if missed |
| `high` | Important outcome, deadline this week, significant impact |
| `medium` | Standard importance, deadline this month |
| `low` | Nice to have, no hard deadline, minimal impact |

---

### Full Task Schema

```json
{
  "id": "task_[unique_id]",
  "title": "Brief, action-oriented task name",
  "description": "Full context of what needs to be done and why",
  "acceptance_criteria": "What done looks like",

  "project_id": "proj_[id] or null",
  "parent_task_id": "task_[id] or null for subtasks",
  "subtasks": [],

  "owner": "person responsible for completing this",
  "created_by": "who created this task",
  "reviewers": [],

  "priority": "critical|high|medium|low",
  "status": "backlog|ready|in_progress|waiting|blocked|in_review|completed|cancelled|deferred",
  "effort": "small|medium|large|xl",

  "created_at": "ISO8601 timestamp",
  "updated_at": "ISO8601 timestamp",
  "started_at": "ISO8601 timestamp or null",
  "due_date": "YYYY-MM-DD or null",
  "completed_at": "ISO8601 timestamp or null",
  "deferred_to": "YYYY-MM-DD or null",

  "dependencies": ["task_id_1", "task_id_2"],
  "blockers": [
    {
      "blocker_id": "block_[id]",
      "description": "What is blocking this task",
      "blocker_owner": "who can resolve this",
      "reported_at": "timestamp",
      "resolved_at": null
    }
  ],

  "next_action": "The specific next thing that needs to happen",
  "source": "conversation|document|external_system",
  "source_reference": "Quote or link to source",
  "evidence": "Proof that this was completed",

  "history": [],
  "notes": [],
  "tags": []
}
```

---

## Commitment Intelligence System

### What Is a Commitment?

A commitment is any explicit or implicit promise, obligation, or responsibility:
- Made by the user to another person
- Made by another person to the user
- Made by a third party to a third party (relevant to a project)

### Commitment Classification

| Type | Description | Example |
|---|---|---|
| `user_committed` | User promised to do something | "I'll send the report by Friday" |
| `received_commitment` | Someone else promised the user something | "John said he'll finish the API by next week" |
| `team_commitment` | A team or group committed to something | "The dev team committed to launching before Christmas" |
| `external_commitment` | External party committed to something | "The vendor promised delivery by Nov 30" |

### Commitment Tracking Rules

**Rule 1 — Accurate Ownership**
Never assign another person's commitment to the user.
Never assign the user's commitment to another person.
Ownership is derived from explicit context, not assumption.

**Rule 2 — Evidence Requirement**
A commitment is only marked COMPLETE when:
- The user explicitly confirms it was done, OR
- A verifiable action proves completion, OR
- The committed party confirms completion

**Rule 3 — Deadline Tracking**
When a deadline passes for an open commitment:
1. Do NOT assume it was completed
2. Do NOT assume it failed
3. Flag it as OVERDUE
4. Note that status requires verification
5. When relevant, surface it to the user

**Rule 4 — Change Tracking**
When a commitment changes (new deadline, changed scope):
1. Preserve original commitment in history
2. Update current record
3. Note when and by whom the change was made

---

### Full Commitment Schema

```json
{
  "id": "commit_[unique_id]",
  "type": "user_committed|received_commitment|team_commitment|external_commitment",
  "description": "What was promised",
  "detailed_scope": "Full context of the commitment",

  "made_by": "who made this commitment",
  "made_to": "who it was made to",
  "owner": "who is responsible for executing it",

  "project_id": "proj_[id] or null",
  "related_tasks": ["task_id_1"],

  "status": "pending|in_progress|completed|overdue|cancelled|disputed",
  "priority": "critical|high|medium|low",

  "created_at": "ISO8601 timestamp",
  "updated_at": "ISO8601 timestamp",
  "committed_on": "when the commitment was made",
  "due_date": "YYYY-MM-DD or null",
  "completed_at": "ISO8601 timestamp or null",

  "evidence": "Quote from conversation or document proving this commitment",
  "completion_evidence": "Proof of completion if applicable",
  "confidence": "verified|probable|assumed",

  "history": [],
  "notes": []
}
```

---

## Open Loops Register

The Open Loops Register tracks everything that is started but not finished.

### Categories of Open Loops

**Category 1 — Unfinished Tasks**
Tasks in `in_progress`, `waiting`, or `blocked` status.

**Category 2 — Unresolved Decisions**
Important choices that have been raised but not made.

**Category 3 — Pending Commitments**
Promises by any party that haven't been confirmed as complete.

**Category 4 — Unanswered Questions**
Questions raised in conversation that need an answer to proceed.

**Category 5 — Missing Dependencies**
Things that need to happen before other work can proceed.

**Category 6 — Planned but Unstarted**
Tasks or projects that were planned but have no progress yet.

### Open Loop Register Format

```markdown
# OPEN LOOPS REGISTER
*Last updated: [timestamp]*

## Unfinished Tasks
| ID | Task | Owner | Status | Due | Priority |
|---|---|---|---|---|---|
| task_045 | API integration | Dev team | BLOCKED | 2026-12-20 | HIGH |

## Unresolved Decisions
| ID | Decision | Stakeholders | Raised | Urgency |
|---|---|---|---|---|
| dec_pending_01 | Choose deployment platform | User, Tech lead | 2026-11-01 | HIGH |

## Pending Commitments
| ID | Commitment | Owner | Due | Type |
|---|---|---|---|---|
| commit_012 | Design mockups | Sarah | 2026-11-10 | received |

## Unanswered Questions
| ID | Question | Context | Raised |
|---|---|---|---|
| q_001 | What is the target user age range? | Mobile app project | 2026-10-20 |

## Missing Dependencies
| ID | Dependency | Blocks | Owner | Status |
|---|---|---|---|---|
| dep_001 | Backend API v2 | Mobile app launch | Backend team | IN PROGRESS |
```

---

## Priority Intelligence

### Next Action Selection Algorithm

When determining "What should I do next?":

```
1. Filter all tasks by status: ready or in_progress
2. For each task, calculate priority score:
   
   SCORE = Priority_Weight + Urgency_Weight + Dependency_Weight + Complexity_Weight
   
   Priority_Weight:
     critical = 100
     high     = 75
     medium   = 50
     low      = 25
   
   Urgency_Weight (days until due):
     <= 1 day  = +50
     <= 3 days = +35
     <= 7 days = +20
     <= 14 days = +10
     > 14 days = 0
     overdue   = +60
   
   Dependency_Weight:
     Blocks 3+ other tasks = +30
     Blocks 1-2 other tasks = +15
     Blocks nothing = 0
   
   Complexity_Weight (inverse — do simpler things when blocked):
     small = +5
     medium = 0
     large = -5
     xl = -10

3. Sort by SCORE descending
4. Return top 3-5 with rationale
```

---

## Commitment Accountability Reporting

When generating commitment status:

```markdown
# COMMITMENT ACCOUNTABILITY REPORT
*Generated: [timestamp]*

## YOUR OPEN COMMITMENTS
| Commitment | To | Due | Status | Days Remaining |
|---|---|---|---|---|
| Send revised proposal | Client X | 2026-11-10 | pending | 2 days |

## COMMITMENTS MADE TO YOU
| Commitment | By | Due | Status | Days Remaining |
|---|---|---|---|---|
| API documentation | John | 2026-11-15 | in_progress | 7 days |

## OVERDUE ITEMS
| Item | Owner | Was Due | Status |
|---|---|---|---|
| Legal review of ToS | Legal team | 2026-11-01 | OVERDUE — status unverified |

*Overdue items require status verification. No completion assumed.*
```

---

*Reference document for THE YOUNGSTER v1.0 — Task & Commitment System*
