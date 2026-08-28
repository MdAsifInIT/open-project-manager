# Wayfinder Ambiguity Framework

Structure complex, undefined, or high-risk architectural decisions.

## Coordinated Files

- **`SPEC.md`**: Target destination (requirements, boundaries, constraints).
- **`MAP.md`**: Active decision graph (`Frontier`, `Blocked`, `Fog`, `Completed`).
- **`ANSWERS.md`**: Durable decision log with rationale and revisit criteria.

---

## Question Lifecycle

1. **`Fog`**: In-scope areas too vague to formulate as crisp questions.
2. **`Blocked`**: Questions waiting on prior decisions.
3. **`Frontier`**: Actionable questions ready for immediate investigation.
4. **`Completed`**: Resolved questions recorded in `ANSWERS.md`.

---

## Decision Record Format (`ANSWERS.md`)

```markdown
### Q-001: [Question Title]
- **Decision:** Chosen architecture or approach.
- **Rationale:** Why this choice over alternatives.
- **Evidence:** Benchmark, spike code, or doc link.
- **Consequences:** Downstream technical impact.
- **Revisit When:** Explicit conditions triggering re-evaluation.
```
