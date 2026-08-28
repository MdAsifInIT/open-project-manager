# Wayfinder Ambiguity Framework

Structure complex, undefined, or high-risk architectural decisions using three coordinated files.

## Coordinated Files

- **`SPEC.md`**: Target destination — goals, scope boundaries, testable requirements, constraints.
- **`.opm/MAP.md`**: Active decision graph categorizing questions by readiness.
- **`.opm/ANSWERS.md`**: Durable decision log with rationale, evidence, and revisit criteria.

---

## Invocation Modes

### Chart
Use when entering a new, undefined problem space. Create `.opm/MAP.md` with initial `Fog` entries for all known unknowns, then decompose into `Frontier` questions.

### Advance
Pick the next unblocked `Frontier` question, investigate it (read code, run experiments, consult docs), and record the resolution in `.opm/ANSWERS.md`. Update `.opm/MAP.md` to reflect newly unblocked questions.

### Refresh
Use when unexpected findings or requirement changes invalidate earlier decisions. Re-evaluate `Completed` questions against new context, downgrade to `Frontier` if the decision no longer holds.

---

## Question Lifecycle

1. **`Fog`**: In-scope areas too vague to formulate as crisp questions. Decompose into concrete questions before proceeding.
2. **`Blocked`**: Questions that depend on an unanswered prior decision. List the blocking question ID explicitly.
3. **`Frontier`**: Precise, actionable questions with no unresolved dependencies. These are the next candidates for investigation.
4. **`Completed`**: Resolved questions with a full entry in `ANSWERS.md`.

---

## Mapping Rules

- Every question gets a permanent ID (`Q-001`, `Q-002`, ...).
- A question moves from `Fog` → `Frontier` only when it can be stated as a single, testable decision.
- A question moves from `Blocked` → `Frontier` only when all blocking questions are `Completed`.
- Do not implement the destination during planning. Wayfinder produces decisions, not code.
- When a `Frontier` question is resolved, check if any `Blocked` questions are now unblocked.

---

## Question Entry Format (`.opm/MAP.md`)

```markdown
### Q-001: Which storage engine for event history?
- **Status:** Frontier
- **Dependencies:** None
- **Context:** Need sub-10ms reads for recent events, append-heavy writes.
```

## Decision Record Format (`.opm/ANSWERS.md`)

```markdown
### Q-001: Which storage engine for event history?
- **Decision:** PostgreSQL with BRIN indexes on timestamp.
- **Rationale:** Sub-10ms reads confirmed in spike. Append-only workload aligns with BRIN.
- **Evidence:** Spike branch `spike/event-store-pg`, benchmark results in PR #42.
- **Consequences:** Requires pg 15+ for BRIN merge. Rules out DynamoDB for this layer.
- **Revisit When:** Write volume exceeds 50k events/sec or read pattern changes to random access.
```

---

## Validation Checklist

- [ ] Every `Fog` entry has been decomposed or explicitly deferred with rationale.
- [ ] Every `Blocked` entry lists its blocking question ID.
- [ ] Every `Completed` entry has a matching `.opm/ANSWERS.md` record.
- [ ] `SPEC.md` is updated to reflect resolved decisions.
- [ ] No implementation code was written during a Wayfinder session.
