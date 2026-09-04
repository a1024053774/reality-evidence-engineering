---
name: reality-first-engineering
description: Establish a short, evidence-backed decision gate before architecture or broad implementation when missing facts, requirements, external behavior, or acceptance criteria could change the direction. Use only for consequential or materially uncertain work.
---

# Reality-First Engineering

Use this before architecture or broad implementation when a wrong assumption would cause
meaningful rework, data loss, security exposure, irreversible side effects, or a false
acceptance result. It is a small decision gate, not a second project manager, code review,
test suite, or reason to delay a cheap reversible probe.

Do not use it for a small change with known inputs, contracts, and environment, or for
documentation/formatting-only work.

## Gate protocol

1. **State the decision.** Write the goal, non-goals, acceptance condition, and the exact
   decision that must be made before implementation.
2. **Separate facts from guesses.** Label each relevant item `observed`, `measured`,
   `inferred`, or `unknown`. An implementation assumption is not evidence. For facts likely to
   change, record the source and verification date.
3. **Choose the smallest probe.** For each material unknown that could change the direction,
   define one falsifiable experiment with real inputs, a pass condition, a failure condition, a
   stop condition, an owner, and an evidence location. Test the highest-impact unknowns first;
   stop once the decision is determined instead of building the whole system to discover it.
4. **Gate the direction.** Return `PASS` only when the architecture or plan follows from the
   available evidence.
   - `BLOCKED`: a decisive input, authority, or probe is unavailable, or the evidence disproves
     the proposed direction; no safe direction can be chosen.
   - `INCOMPLETE`: evidence exists but is partial or ambiguous; narrow to verified work or
     report what remains, and never silently treat it as `PASS`.
   Do not cover the gap with a fallback, compatibility path, mock-only branch, or speculative
   abstraction.
5. **Keep the boundary testable.** Put external effects behind the existing boundary that
   owns them and keep deterministic domain/state logic driven by supplied inputs. A mock may
   validate pure logic, but it cannot establish an external fact it replaces. Add a layer only
   when a concrete caller or boundary needs it.

## Keep one source of truth

If the work already has a project plan, issue, decision record, or task note, update that source
instead of creating a second ledger. For one-off work, keep the brief in the task response or
the existing work record. Do not create a separate reality document solely to use this Skill.

Run one gate per decision batch. Edits and test iterations under the same assumptions do not
restart it. Re-open only when new evidence invalidates an assumption, scope changes, or a new
direction is proposed.

## Required brief

```text
REALITY GATE: PASS | BLOCKED | INCOMPLETE
GOAL / NON-GOALS:
DECISION:
OBSERVED:
MEASURED:
INFERRED:
UNKNOWN (material only):
PROBE (repeat for each material unknown): input/context, method, pass/fail,
stop condition, owner, evidence location
DECISION CONSEQUENCE:
NEXT ACTION / OWNER:
```

After `PASS`, derive the smallest implementation plan and use the project's normal tests and
review process. This gate does not replace behavioral testing or review. The honest result may
be a stop: a clean `BLOCKED`/`INCOMPLETE` with a precise next probe is better than a polished
implementation of an unverified premise.
