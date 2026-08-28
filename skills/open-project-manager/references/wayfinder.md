# Wayfinder Ambiguity Framework

Turn vague, complex, or high-risk architectural destinations into structured, durable planning records.

## Core Concepts

Wayfinder structures problem spaces using three coordinated files:

- **`SPEC.md`**: The agreed destination — goals, scope boundaries, testable requirements, and constraints.
- **`MAP.md`**: The live decision route — categorizes questions into `Frontier`, `Blocked`, `Fog`, and `Completed`.
- **`ANSWERS.md`**: The durable decision record — stores rationale, evidence, trade-offs, and revisit conditions for each resolved question.

---

## Invocation Modes

- **Chart**: Use when encountering a new, undefined problem space or when starting a complex subsystem from scratch.
- **Advance**: Use to pick the next unblocked question from the `Frontier`, investigate, and record the resolution.
- **Refresh**: Use when unexpected findings or changes in requirements invalidate earlier decisions.

---

## Question Lifecycle

1. **`Fog`**: In-scope problem areas that are too hazy to formulate as crisp questions.
2. **`Blocked`**: Questions that depend on another unanswered decision.
3. **`Frontier`**: Precise, actionable questions with no unresolved dependencies.
4. **`Completed`**: Resolved questions with a full entry in `ANSWERS.md` and settled outcomes recorded in `SPEC.md`.

---

## Question Entry Standard

Assign each question a permanent identifier (`Q-001`, `Q-002`):

```markdown
### Q-001: Which storage engine should be used for event history?
- **Status:** Completed
- **Dependencies:** None
- **Resolution:** See [ANSWERS.md#Q-001](ANSWERS.md#Q-001)
```

In `ANSWERS.md`, record:
- **Decision:** The chosen path.
- **Rationale:** Why this choice was made over alternatives.
- **Evidence:** Code tests, performance benchmarks, or documentation links.
- **Consequences:** Downstream effects on architecture.
- **Revisit When:** Explicit conditions under which this decision should be re-evaluated.
