# Retrieval Strategy Reference
## THE YOUNGSTER — Eternal Memory & Continuity Intelligence

---

## The Retrieval Imperative

Memory without retrieval is worthless. THE YOUNGSTER's retrieval system is engineered to surface the RIGHT information at the RIGHT time — no noise, no gaps, no fabrications.

**Core Principle:** Never return irrelevant information. Never withhold relevant information. Never invent information.

---

## Nine Retrieval Modes

### MODE 1 — DIRECT RETRIEVAL
**Trigger:** User asks for a specific fact or record.
**Example:** "What was the last decision we made?"

**Process:**
1. Identify the record type being requested (decision)
2. Query the most recent record of that type
3. Return with timestamp, source, and confidence level

**Output format:**
```
Last decision: [Decision title]
Made: [Date]
Rationale: [Why this was chosen]
Source: [Conversation/document reference]
Confidence: [verified/probable/assumed]
```

---

### MODE 2 — PROJECT RETRIEVAL
**Trigger:** User asks about a specific project.
**Example:** "Where did we leave the website project?"

**Process:**
1. Find the project record by name or keyword
2. Load the latest project snapshot
3. Load active tasks and their status
4. Load recent decisions
5. Identify blockers and dependencies
6. Determine next logical action

**Output:** Full continuity package (see references/project-continuity.md)

---

### MODE 3 — HISTORICAL RETRIEVAL
**Trigger:** User asks about past events or decisions.
**Example:** "What did we do in the first phase?"

**Process:**
1. Query records filtered by: `completed_at` is not null, or `status: completed/archived`
2. Apply date filters if mentioned
3. Apply project filters if mentioned
4. Return in chronological order
5. Include evidence where available

**Critical Rule:** Return ACTUAL historical records. Never reconstruct from inference what should have happened.

---

### MODE 4 — FUTURE RETRIEVAL
**Trigger:** User asks about upcoming work.
**Example:** "What do I have coming up this month?"

**Process:**
1. Query all records with `due_date` in the future
2. Filter by current month if "this month" mentioned
3. Separate confirmed from proposed/intentions
4. Sort by urgency (soonest due date first)
5. Include commitment type (user vs. received)

**Output format:**
```
UPCOMING — [Month/Period]

Confirmed:
  [date] — [task/commitment] — Owner: [person]
  
Proposed (not yet confirmed):
  [date] — [task/plan] — Status: proposed

Intentions (casual, unconfirmed):
  [reference] — mentioned as possible future activity
```

---

### MODE 5 — CROSS-CONTEXT RETRIEVAL
**Trigger:** User asks how current work connects to past work.
**Example:** "How does this new feature relate to what we built before?"

**Process:**
1. Extract key entities from the current context (feature name, project, technology)
2. Search all records for related entities
3. Identify connections by: shared project, shared technology, shared stakeholders, shared decisions
4. Surface only genuinely relevant connections

**Output:** Relationship map with explanation of how items connect.

---

### MODE 6 — RESUMPTION RETRIEVAL
**Trigger:** User wants to continue previous work.
**Example:** "Let's continue." / "Where were we?"

**Process:**
1. Identify the most recently active project or task
2. Build the full continuity package
3. Identify the precise next step
4. Surface any blockers or pending decisions
5. Present in action-ready format

**Output:** "Here's exactly where we were and what happens next."

---

### MODE 7 — COMMITMENT RETRIEVAL
**Trigger:** User asks about responsibilities and obligations.
**Example:** "What have I promised to do?" / "What is John responsible for?"

**Process:**
1. Query commitment records filtered by owner (if specified) or all parties
2. Filter by status: pending, overdue
3. Sort by urgency (due date)
4. Separate user commitments from received commitments

**Output:** Commitment accountability table (see task-and-commitment-system.md)

---

### MODE 8 — CHANGE RETRIEVAL
**Trigger:** User asks what changed between sessions or time periods.
**Example:** "What changed since last time?"

**Process:**
1. Identify the comparison period (last session vs. now, specific date range)
2. Query all records with `updated_at` in that period
3. Identify status changes (compare history entries)
4. Identify new records created
5. Identify completed records

**Output:** Temporal change report (see temporal-reasoning.md)

---

### MODE 9 — GAP DETECTION
**Trigger:** User asks what is missing or unclear.
**Example:** "What don't we know yet?" / "What's still unclear?"

**Process:**
1. Scan all active project records for empty required fields
2. Identify decisions marked as `pending` for extended periods
3. Identify tasks with no defined owner
4. Identify tasks with no defined next action
5. Identify dependencies with unknown status
6. Surface unanswered questions

**Output:**
```
KNOWLEDGE GAPS — [Project]

Missing Information:
  - Target user age range: not defined
  - Deployment platform: decision pending since Oct 20

Unowned Tasks:
  - task_078: Security audit — no owner assigned

Undefined Next Actions:
  - proj_003: Backend API — in progress but no next step documented

Outstanding Questions:
  - What is the monthly active user target?
  - Is multi-language support in scope for v1?
```

---

## Retrieval Priority Rules

When multiple items match a retrieval request:

```
PRIORITY ORDER:

1. Exact match to user's stated request
2. Current project context (what we're currently working on)
3. Latest verified state (most recent, most confident)
4. Relevant historical decisions (that explain current situation)
5. Outstanding commitments (pending action items)
6. Upcoming dependencies (things blocking future work)
7. Relevant long-term preferences (user working style)
```

---

## Retrieval Quality Rules

### Anti-Noise Rules
- Never return records with score < 50 for importance unless specifically requested
- Never return more records than necessary to answer the question
- Never surface project history when only current state was asked for
- Never mix confirmed facts with assumptions without labeling them

### Anti-Gap Rules
- Never omit relevant blockers from project status
- Never omit overdue items from future retrieval
- Never omit unresolved conflicts from change reports
- Always label what is unknown as unknown

### Confidence Labeling
Every retrieved piece of information must carry a confidence label:
```
[VERIFIED] — Confirmed by user or authoritative source
[PROBABLE] — Strong evidence but not explicitly confirmed
[ASSUMED] — Inferred from context, not directly stated
[UNCERTAIN] — Conflicting information or low-confidence source
```

---

## Search & Matching Logic

### Exact Match
When user mentions a specific project name, task name, or record ID:
- Match on name (case-insensitive)
- Match on aliases or shortened versions
- Match on project code

### Semantic Match
When user describes something without naming it:
- Extract key entities (technology, person, date, deliverable)
- Match against record titles, descriptions, and tags
- If multiple matches: return top 3 with disambiguation question

### Fuzzy Match
When exact or semantic match fails:
- Broaden search to related projects
- Search in archived records
- Surface the best available match with note: "This may be what you're looking for — confirm if correct"

### No Match
When nothing matches:
- State clearly: "I don't have a record matching [description]"
- Do NOT invent a matching record
- Do NOT return an unrelated record as a substitute
- Suggest what information is needed to create a new record

---

## Anti-Fabrication Enforcement

**The most important retrieval rule:**

> If the information is not in a verified record, THE YOUNGSTER does not make it up.

Fabrication scenarios to guard against:
1. **Hallucinated completion** — Claiming a task is done because it feels logical it should be done
2. **Invented deadline** — Creating a date when none was specified
3. **Assumed decision** — Presenting an inferred choice as a confirmed decision
4. **False continuity** — Reconstructing project history from inference rather than records
5. **Confidence inflation** — Presenting uncertain information as verified

When in doubt:
> "I don't have a verified record of [X]. Can you confirm what the actual status is?"

---

*Reference document for THE YOUNGSTER v1.0 — Retrieval Strategy*
