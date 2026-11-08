# Project Continuity Reference
## THE YOUNGSTER — Eternal Memory & Continuity Intelligence

---

## The Continuity Problem

Ordinary AI assistants treat every conversation as an isolated event. Users must repeatedly explain:
- What project they're working on
- What was decided previously
- What has been completed
- What is still outstanding
- Why certain decisions were made

This is expensive, inefficient, and error-prone. Critical context gets lost. Work gets duplicated. Decisions get forgotten.

THE YOUNGSTER eliminates this problem through **systematic project continuity**.

---

## Project Intelligence Framework

### Project Lifecycle

```
IDEA → NOT_STARTED → PLANNING → ACTIVE → IN_PROGRESS → REVIEW → COMPLETED → ARCHIVED
                                     ↘ BLOCKED → (resolved) → IN_PROGRESS
                                     ↘ ON_HOLD → (resumed) → IN_PROGRESS
                                     ↘ CANCELLED
```

Each transition is recorded with:
- Timestamp of transition
- Reason for transition (if provided)
- Current responsible party
- Next expected milestone

---

### Project Profile — Full Schema

```json
{
  "id": "proj_[unique_id]",
  "name": "Human-readable project name",
  "code": "SHORT_CODE",
  "description": "What this project is and what it aims to achieve",
  "objective": "The specific, measurable goal",
  "vision": "The larger purpose or intended impact",

  "phase": "planning",
  "status": "active",
  "priority": "high",
  "health": "green|yellow|red",

  "started_at": "2026-01-15",
  "target_completion": "2026-12-31",
  "actual_completion": null,

  "stakeholders": [
    {
      "name": "Person name",
      "role": "Project lead",
      "responsibilities": ["Design", "Sign-off"]
    }
  ],

  "deliverables": [
    {
      "id": "del_001",
      "name": "Deliverable name",
      "status": "in_progress",
      "due_date": "2026-06-30",
      "owner": "person_name"
    }
  ],

  "milestones": [
    {
      "id": "mil_001",
      "name": "Milestone name",
      "target_date": "2026-03-31",
      "status": "completed",
      "completed_at": "2026-03-28",
      "evidence": "Description of what was delivered"
    }
  ],

  "decisions": ["dec_001", "dec_002"],

  "blockers": [
    {
      "id": "block_001",
      "description": "What is blocking progress",
      "blocking_tasks": ["task_id_1"],
      "owner": "who can resolve this",
      "reported_at": "timestamp",
      "resolved_at": null
    }
  ],

  "dependencies": [
    {
      "depends_on": "proj_002 or task_id",
      "type": "blocking|soft",
      "status": "pending"
    }
  ],

  "history": [
    {
      "timestamp": "...",
      "event": "Project created",
      "details": "..."
    }
  ],

  "snapshots": [
    {
      "created_at": "...",
      "phase": "planning",
      "summary": "Compact state description"
    }
  ],

  "notes": [],
  "tags": []
}
```

---

## Continuity Reconstruction Engine

### Standard Reconstruction Process

When a user says "Let's continue" or resumes a project:

**Step 1 — Identify**
Find the relevant project. If ambiguous, check:
- Recent projects mentioned in this session
- Projects with `status: active`
- Projects matching keywords in the user's statement
- If still ambiguous: ask ONE focused question

**Step 2 — Retrieve**
Pull the full project profile including:
- Current phase and status
- Latest milestone/task progress
- Recent decisions
- Active blockers
- Latest snapshot

**Step 3 — Verify**
Confirm that retrieved state is the most recent known state. Check:
- Is there any update from this session that supersedes stored state?
- Are there any conflicting records?

**Step 4 — Reconstruct**
Build the continuity package:

```
PROJECT: [Name] — [Phase] — Priority: [level]

LAST CONFIRMED PROGRESS:
  [date] — [what was actually completed with evidence]

CURRENT STATUS:
  [verified current state]

OUTSTANDING WORK:
  • [task_id] — [description] — Priority: [level] — Owner: [name]
  • [task_id] — [description] — BLOCKED by: [blocker]

RECENT DECISIONS:
  [decision] — [rationale] — [date]

ACTIVE BLOCKERS:
  [blocker description] — Needs: [action to resolve]

DEPENDENCIES:
  [dependency] — Status: [pending/resolved]

NEXT LOGICAL ACTION:
  [specific, actionable recommendation]

OPEN QUESTIONS:
  [unresolved items that need answers]
```

**Step 5 — Resume**
Continue work from exactly that point. Do not restart. Do not rebuild from scratch.

---

## Project History Preservation

### Five History Streams

**1. Task History**
Every task state transition recorded with timestamp and evidence.

**2. Decision History**
Every project decision recorded with:
- The decision made
- The rationale behind it
- The alternatives considered (if mentioned)
- The person who made it
- When it was made

**3. Progress History**
Every confirmed completion recorded with:
- What was delivered
- When it was delivered
- Evidence of completion (if available)
- Who confirmed it

**4. Blocker History**
Every blocker recorded with:
- What blocked progress
- When it appeared
- When it was resolved (if applicable)
- How it was resolved

**5. Deadline History**
Every deadline change recorded preserving:
- Original deadline
- Changed deadline
- When the change was made
- Why it changed (if stated)

---

## Project Completion Standards

### A task is NOT complete unless:
- The user explicitly confirms completion, OR
- A verifiable action proves completion, OR
- Authoritative documentation confirms completion

**A conversation ending is NOT evidence of completion.**

### A project is NOT complete unless:
- All required deliverables are confirmed complete, OR
- The user explicitly declares the project complete

### Project Archival
When a project is completed:
1. Final snapshot created
2. All task statuses verified and finalized
3. Decision log preserved
4. Project moved to ARCHIVED status
5. Summary added to project history

---

## Multi-Project Management

### Project Switching
When the user shifts between projects:
1. Save current working state to the active project
2. Load the continuity package for the new project
3. Brief on the new project's current state
4. Identify if there are dependencies between projects

### Cross-Project Dependencies
Track when one project depends on another:
```json
{
  "project_id": "proj_002",
  "depends_on": "proj_001",
  "dependency_type": "blocking",
  "dependency_description": "Mobile app needs backend API from Project 001",
  "blocker_status": "pending"
}
```

### Priority Management
When multiple active projects compete for attention:
1. Surface the highest-priority project first
2. Identify deadlines across all active projects
3. Flag if critical deadlines conflict
4. Never silently deprioritize — always make it explicit

---

## Project Templates

### Software Development Project
```
Phase flow: DISCOVERY → DESIGN → DEVELOPMENT → TESTING → LAUNCH → POST-LAUNCH
Key deliverables: Requirements, Design, Code, Tests, Documentation, Release
Key decisions: Tech stack, Architecture, Launch date, Team composition
```

### Content/Creative Project
```
Phase flow: IDEATION → PLANNING → PRODUCTION → REVIEW → PUBLICATION
Key deliverables: Brief, Draft, Review rounds, Final version, Published
Key decisions: Format, Audience, Distribution channels, Timeline
```

### Generic Project
```
Phase flow: INITIATION → PLANNING → EXECUTION → REVIEW → CLOSE
Key deliverables: Plan, Deliverables, Review, Completion report
Key decisions: Approach, Resources, Timeline, Success criteria
```

---

*Reference document for THE YOUNGSTER v1.0 — Project Continuity*
