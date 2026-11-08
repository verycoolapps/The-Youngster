# Memory Architecture Reference
## THE YOUNGSTER — Eternal Memory & Continuity Intelligence

---

## Overview

THE YOUNGSTER's memory architecture is built on three foundational principles:

1. **Truth over completeness** — A partial but honest memory beats a fabricated complete one.
2. **History preservation** — Nothing important is ever silently deleted.
3. **Provenance tracking** — Every record knows where it came from.

---

## The Three-Dimensional Memory Model

### Dimension A: PAST MEMORY

**Purpose:** Understand what happened and why.

**Stores:**
- Completed projects and their outcomes
- Historical decisions with rationale
- Previous versions of plans and requirements
- Problems encountered and their solutions
- Mistakes made and lessons extracted
- Previous user preferences and working styles
- Historical project versions and their states

**Record Requirements:**
Every PAST record must include:
- `id` — unique record identifier
- `event_description` — what actually happened
- `occurred_at` — when it happened
- `project_id` — related project
- `people_involved` — who was part of it
- `outcome` — what resulted
- `source` — where the information came from
- `evidence` — supporting quotes or references
- `confidence` — how certain we are this is accurate

**Golden Rule:** Historical facts are never rewritten because the current situation changed. History is immutable. Only current state changes.

---

### Dimension B: PRESENT MEMORY

**Purpose:** Maintain accurate awareness of what is happening right now.

**Stores:**
- Active projects and their current phase
- Current tasks and their status
- Current priorities (ranked)
- Active blockers
- Pending decisions awaiting resolution
- Active deadlines
- In-progress deliverables
- Current responsibilities by owner
- Recently changed requirements

**Update Protocol:**
When present state changes:
1. Identify the existing record
2. Update the current fields
3. Write the old state to `history[]`
4. Set `updated_at` to current timestamp
5. Record the source of the update

**Latest State Rule:** Always identify the LATEST VERIFIED state before answering continuity-dependent questions. Never assume current state from past information.

---

### Dimension C: FUTURE MEMORY

**Purpose:** Track what is planned, promised, and expected.

**Stores:**
- Planned activities with proposed dates
- Upcoming milestones
- Confirmed deadlines
- Promised deliverables by owner
- Dependencies not yet resolved
- Long-term objectives
- Expected outcomes

**Temporal Classification System:**

| Classification | Trigger | Behavior |
|---|---|---|
| `intention` | "maybe", "probably", "might" | Record but do not treat as plan |
| `proposal` | "we should", "let's plan to" | Record as proposal, needs confirmation |
| `plan` | "we will", "the plan is" | Record as plan, still needs confirmation |
| `confirmed` | "we have agreed", "it's scheduled", explicit commitment | Full commitment record |
| `scheduled` | Specific calendar date/time given | Deadline record created |

**Critical Rule:** A casual statement like "maybe next month" NEVER automatically becomes a confirmed task. Future information must have an appropriate status and confidence level.

---

## Memory Storage Layers

### Layer 1: Durable Persistent Storage
**Mechanism:** Auto-memory files in persistent memory directory
**Durability:** Survives session end
**Access:** Available in future conversations
**Use for:** Projects, decisions, commitments, completed work

**Write Protocol:**
```
1. Validate the record schema
2. Check for existing duplicate records
3. Write to persistent storage
4. Confirm write success
5. Add to memory index
6. Update any linked records
```

### Layer 2: Structured Project Ledgers
**Mechanism:** Markdown files and JSON in project directories
**Durability:** Survives session end
**Access:** Searchable and human-readable
**Use for:** Project profiles, task lists, decision logs

### Layer 3: Session Working Memory
**Mechanism:** Conversation context
**Durability:** Current session only
**Access:** Immediate
**Use for:** Temporary context, in-progress work, real-time tracking

**Warning Label:** Always explicitly label when information is session-only. Never claim it is persisted when it is not.

---

## Memory Index Structure

The memory index provides fast lookup without scanning all records:

```markdown
# YOUNGSTER MEMORY INDEX

## Projects
- [proj_001] Website Redesign — ACTIVE — Last updated: 2026-11-08
- [proj_002] Mobile App v2 — BLOCKED — Last updated: 2026-11-05

## Active Decisions
- [dec_001] PostgreSQL chosen over MySQL — 2026-10-15
- [dec_002] Launch date moved to Q1 2027 — 2026-11-01

## Open Commitments
- [commit_001] John to deliver API docs by 2026-11-15
- [commit_002] User to review design mockups by 2026-11-10

## Recent Completions
- [task_033] Homepage redesign — COMPLETED — 2026-11-07
- [task_034] Database schema v2 — COMPLETED — 2026-11-06

## Open Loops
- [loop_001] Q3 budget approval — WAITING — owner: Finance team
- [loop_002] Legal review of ToS — PENDING — no owner assigned
```

---

## Knowledge Graph

Tracks relationships between all entities:

```
Project ──────────────── Task
   │                      │
   ├── Decision           ├── Owner
   ├── Milestone          ├── Blocker
   ├── Stakeholder        └── Dependency
   └── Deadline

Commitment ─────────────── Person
   │                         │
   ├── Owner                 ├── Project role
   └── Due date              └── Commitments made
```

**Relationship Rules:**
- Every task links to a project (or is flagged as orphan)
- Every commitment has an identified owner
- Every decision links to its project and affected tasks
- Every blocker links to the task(s) it blocks

---

## Memory Health Checks

Run when explicitly requested or when anomalies are detected:

| Check | What It Validates |
|---|---|
| Completeness | Are critical fields populated? |
| Currency | Is the latest state reflected? |
| Consistency | Do linked records agree? |
| Retrievability | Can records be found through the index? |
| Duplicate detection | Are there redundant records? |
| Conflict identification | Are there contradicting facts? |
| Orphan detection | Are there tasks with no project? |
| Stale detection | Are there records with no recent update? |

---

## Append-Oriented History Model

THE YOUNGSTER never destroys history. Every important change is preserved.

**Example — Deadline Change:**

```json
{
  "id": "task_045",
  "title": "API integration complete",
  "due_date": "2026-12-20",
  "updated_at": "2026-11-08T14:30:00Z",
  "history": [
    {
      "timestamp": "2026-10-01T09:00:00Z",
      "field_changed": "due_date",
      "old_value": null,
      "new_value": "2026-12-10",
      "source": "conversation",
      "changed_by": "user"
    },
    {
      "timestamp": "2026-11-08T14:30:00Z",
      "field_changed": "due_date",
      "old_value": "2026-12-10",
      "new_value": "2026-12-20",
      "source": "conversation",
      "changed_by": "user"
    }
  ]
}
```

This preserves both the original deadline (December 10) and the new deadline (December 20), with timestamps for when each was set.

---

*Reference document for THE YOUNGSTER v1.0 — Memory Architecture*
