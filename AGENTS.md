# Agent Skills Lab Instructions

## Repository Purpose

This repository develops and evaluates production-oriented Agent Skills.

The goal is not to build a prompt collection or a general Agent framework. Each Skill must represent a focused, reusable workflow that changes Agent behavior in a measurable way.

The current active Skill is:

* `execute-repo-handoff`

Do not start additional Skills unless explicitly requested.

## Repository Layout

Root-level Skill directories are the source of truth.

Example:

```text
execute-repo-handoff/
  SKILL.md
  evals/
```

`.agents/skills/` exists only for Codex repository-local Skill discovery.

Skill entries under `.agents/skills/` must point to the corresponding root-level Skill directory. Do not maintain duplicate Skill content under `.agents/skills/`.

## Current Development Scope

Current work is limited to the first usable version of `execute-repo-handoff`.

Priorities:

1. Define the Skill boundary and execution contract.
2. Build the initial evaluation cases.
3. Establish a no-Skill baseline.
4. Write the smallest `SKILL.md` that addresses observed failures.
5. Evaluate behavioral and efficiency changes.
6. Add references or scripts only when repeated failures justify them.

Do not add speculative infrastructure.

In particular, do not introduce `scripts/`, `references/`, a Python package, Promptfoo, CI, or a general evaluation framework unless the current task provides a concrete need.

## Skill Authoring Rules

* Every Skill must contain `SKILL.md`.
* Keep each Skill focused on one job.
* Treat `name` and `description` as the routing contract.
* The `description` must state when the Skill should and should not trigger.
* Keep detailed input, execution, validation, failure, and stop rules in the Skill instructions rather than overloading the description.
* Write workflow instructions as imperative, executable steps with explicit inputs and outputs.
* Prefer instructions over scripts.
* Add a deterministic script only when mechanical execution or validation materially improves reliability.
* Load supporting knowledge progressively. Do not move content into `references/` unless it is useful conditionally or makes `SKILL.md` materially clearer.
* Do not duplicate the same authority across multiple files.

## Development Method

Use failure-first, eval-driven Skill development.

The default loop is:

```text
repeated Agent failure
-> define Skill boundary
-> create representative eval cases
-> observe baseline behavior
-> write minimal Skill instructions
-> evaluate behavior
-> fix demonstrated failures
-> stop
```

Do not design infrastructure for hypothetical future failures.

Already established project decisions are authority. Do not repeat broad research or redesign settled choices unless concrete repository evidence contradicts them.

## `execute-repo-handoff` Evaluation Rules

The initial evaluation suite should stay small.

The first three case classes are:

* `valid-minimal`
* `scope-temptation`
* `stale-handoff`

For initial marginal-utility testing, prefer:

```text
B: structured handoff without the Skill
C: the same structured handoff with the Skill
```

The handoff content must remain identical between B and C.

The first question is whether the Skill adds value beyond the structured handoff itself.

Prefer deterministic observations and graders before LLM-as-judge evaluation.

Relevant observations include:

* task success
* handoff validity classification
* files read
* reads outside declared scope
* justified scope expansion
* files modified outside allowed scope
* commands or tool calls
* required validation result
* token usage when reliably available
* duration when reliably available

Do not convert unavailable metrics to zero.

Keep these states distinct:

* task failure
* invalid or stale handoff
* operational failure

Start with explicit Skill invocation when validating behavior. Implicit routing becomes a hard evaluation concern only after triggered behavior is useful.

## Repository Observation Rules

Minimize repository reconnaissance.

When the target is already known, use:

```text
file
-> symbol
-> smallest relevant range
-> resolve
-> stop
```

Do not reread the full repository, full documentation set, or unrelated files when the task already identifies the relevant boundary.

Expand observation only when concrete evidence requires it, such as:

* imports
* direct call relationships
* test failures
* type errors
* repository instructions
* contradiction with the supplied handoff

Do not treat uncertainty alone as justification for broad repository exploration.

## Implementation Rules

* Make the smallest change that satisfies the current task.
* Do not modify unrelated files.
* Do not introduce dependencies without a demonstrated need.
* Do not add abstractions solely for possible future use.
* Do not perform speculative cleanup or unrelated refactoring.
* Preserve existing repository decisions unless current evidence proves them invalid.
* Do not commit or push changes unless explicitly requested.

## Validation Rules

Use the narrowest validation that directly verifies the changed behavior.

Prefer:

```text
direct boundary or unit validation
```

over:

```text
large integration or end-to-end validation
```

when the local boundary is sufficient.

If a validation command is specified by the task, run that exact command.

If no validation infrastructure exists yet, do not invent a large framework merely to claim validation.

Never claim a validation passed if it was not run successfully.

## Stop Rule

When the requested change, focused evaluation, and specified validation are complete, stop.

Do not continue with:

* speculative architecture work
* unrelated refactoring
* additional Skills
* generalized framework construction
* review of unrelated repository areas
