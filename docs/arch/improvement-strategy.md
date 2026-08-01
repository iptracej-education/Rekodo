# Improvement Strategy 

An improvement strategy is a user-space policy that examines a recorded result
and any available evaluation evidence, then proposes what the Rekodo
Orchestrator should do next.

It may propose:

- accepting the result and completing the current work;
- requesting a critique;
- invoking tool-grounded verification;
- requesting a revision;
- retrying with different context;
- assigning the result to another worker;
- escalating the decision to a human;
- proposing memory, a skill, or another artifact for a future run.

The strategy does not contact workers, construct Task Contracts, or write
directly to the canonical history. The orchestrator evaluates the proposal and,
when accepted, runs a Task Contract Program to create the next controlled
assignment.

```text
Recorded result
      ↓
Evaluation evidence
      ↓
Improvement strategy
      ↓
Proposed follow-up
      ↓
Orchestrator decision
      ↓
Task Contract, completion, or escalation
```

## Within-Run Improvement

Within-run strategies attempt to improve the current outcome before the run or
task completes.

Examples include:
```
draft → critique → revision
draft → tool verification → grounded revision
failure → reflection → retry
result → human review → accept or revise
```
Self-Refine and CRITIC primarily operate within a run.

## Across-Run Improvement

Across-run strategies propose findings that may influence future runs.

Examples include:

```
completed run → reflection proposal
failed run → proposed skill update
evaluation evidence → proposed orchestration change
```

A proposal does not automatically become memory, a skill, or project policy.
It must be reviewed and promoted according to user-space policy. Once accepted,
it becomes an explicit, versioned input to a later run.

Rekodo does not learn silently.

## Evidence and Control

An improvement strategy may attempt to improve an outcome, but it does not
define universally what “better” means. Each project supplies its own
objectives, evaluators, acceptance rules, and trade-offs among quality, cost,
latency, and reliability.

An iterative strategy must also define:

- its stopping condition;
- its maximum number of attempts;
- any cost or time limit;
- what happens when no acceptable result is produced.

These controls prevent critique, revision, and retry loops from continuing
without bound.

## Recording

For every use of an improvement strategy, Rekodo records:

- the strategy name, version, and configuration;
- the result and evaluation evidence it consumed;
- the follow-up it proposed;
- the orchestrator’s acceptance, rejection, or modification of that proposal;
- any Task Contract produced as a consequence.

This makes improvement attempts attributable and allows different strategies to
be compared without changing the Rekodo Runtime.

<<: [README](../../README.md) | <: [BACK - Orchestration Patterns](orchestration-patterns.md) | [NEXT - Evaluator](evaluator.md)>:  
