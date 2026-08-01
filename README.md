# Rekodo 

Minimal, improvable orchestration for multi-agent runs. Every run recorded and the next run improved.

Rekodo combines a minimal runtime core with an extensible user-space orchestration framework. 

> [!NOTE]
> This project is in progress. Once an alpha version is completed, the execution code will be published. 

## Core Foundation

Agent context is temporary; Rekodo’s record is not. Even after context compression or restart, the assignments, decisions, and results that pass through Rekodo remain available in one ordered history.

The minimal runtime records, orders, relays, and replays that history. A replaceable user-space orchestrator decides what happens next and may use Task Contract Programs, orchestration patterns, evaluators, memory, skills, and improvement strategies without changing the runtime.

At cold start, Rekodo has no run-derived memory or learned skills. Improvement begins when evaluated findings are explicitly accepted and carried into later runs.

See [Foundation](docs/arch/foundation.md) for the complete design principles.

## Architecture

Rekodo has two architectural layers: the **Rekodo Runtime** and the **Rekodo Orchestrator**. 

![Recode Architecture](./docs/arch/diagram.png)
(By GPT-5.6-sol) 

### Rekodo Runtime

The Rekodo Runtime remains deliberately unintelligent. It understands only generic execution information and is responsible for:

- recording an input before making it available to orchestration;
- recording a Task Contract before delivering it to its declared worker;
- recording a result before relaying it to orchestration;
- assigning a canonical order to recorded events;
- preserving, replaying, and exposing the recorded history.

The runtime does not select tasks or workers, interpret project contracts, construct worker assignments, evaluate results, or decide what happens next. It records, orders, and relays the execution chosen by the orchestrator.

### Rekodo Orchestrator

Rekodo provides a default orchestrator as a separate user-space component. It is replaceable: applications may extend it or bring their own orchestrator as long as they use the Rekodo Runtime protocol.

The orchestrator may combine ordinary program logic with commercial, open-source, or local LLMs. Each run records the orchestrator implementation, version, configuration, and the versions of its selected Task Contract Programs, patterns, strategies, and evaluators.

The orchestration framework separates four areas of capabilities:

- [Task Contract Programs](docs/arch/task-contract-programs.md) — Produce structured, policy-controlled assignments for agents and tools.
- [Orchestration Patterns](docs/arch/orchestration-patterns.md) — Organize roles and tasks into sequential, iterative, or parallel work.
- [Improvement Strategies](docs/arch/improvement-strategy.md) — Decide what should follow a result, such as acceptance, critique, verification, revision, or escalation.
- [Evaluators](docs/arch/evaluators.md) — Supply evidence about correctness, quality, completion, cost, or latency.


### Run Programs and Replay

The Rekodo Orchestration Framework produces an executable Run Program for each
run. A Run Program contains the user-space orchestration logic used to select
tasks, construct Task Contracts, invoke workers and evaluators, and react to
recorded outcomes.

Rekodo records the exact Run Program and its inputs before they affect
execution. Replay executes this recorded program in a compatible environment;
it does not require the original Orchestration Framework implementation that
produced it.

The framework implementation may be recorded for provenance or regeneration,
but the Run Program—not the framework version—is the replay dependency.
Executing the program with recorded external observations reconstructs the
original run. Executing it against live models, tools, or humans creates a new
rerun whose results may differ.

### An Example Run

Here is the simplest run example using Rekodo runtime and Orchestration capabilities: 

```text
User submits a task
        ↓
Runtime records the input
        ↓
Orchestrator selects a worker and orchestration pattern
        ↓
Task Contract Program produces a controlled assignment
        ↓
Runtime records and delivers the Task Contract
        ↓
Worker performs the assignment and returns a result
        ↓
Runtime records and relays the result
        ↓
Evaluator supplies evidence
        ↓
Improvement strategy accepts, revises, retries, escalates, or completes
```

The orchestrator decides what happens. The runtime records and relays what happens.








