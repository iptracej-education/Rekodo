# Rekodo 

Minimal, improvable orchestration for multi-agent runs. Every run recorded and the next run improved.

Rekodo combines a minimal runtime core with an extensible user-space orchestration framework. 

> [!NOTE]
> This project is heavly in progress. Once an alpha version is completed, the execution code will be published. 

## Core Foundation

Rekodo records each run as one ordered, canonical history, so agents and humans share the same record of what was assigned, decided, and returned—even after context compression or restart.

The runtime only records, orders, relays, and replays this history. Planning, Task Contract generation, evaluation, and improvement remain in user space, so they can evolve without changing the runtime.

See [Foundation](docs/arch/foundation.md) for the complete design principles.

## Architecture

Rekodo has two architectural layers: the **Rekodo Runtime** and the **Rekodo Orchestrator**. 

![Recode Architecture](./docs/arch/diagram.png)
(Developed by ChatGPT) 

### Rekodo Runtime

The Rekodo Runtime remains deliberately unintelligent. It understands only generic execution information and is responsible for:

- recording an input before making it available to orchestration;
- recording a Task Contract before delivering it to its declared worker;
- recording a result before relaying it to orchestration;
- assigning a canonical order to recorded events;
- preserving, replaying, and exposing the recorded history.

The runtime does not select tasks or workers, interpret project contracts, construct worker assignments, evaluate results, or decide what happens next. It records, orders, and relays the execution chosen by the orchestrator.

### Rekodo Orchestrator

The Rekodo Orchestrator is the programmable user-space framework where intelligent and project-specific behavior lives. It is not an LLM itself, but it may combine ordinary program logic with commercial, open-source, or local LLMs.

The orchestrator is responsible for:

- selecting the next task and its recipient;
- applying an orchestration pattern;
- running a Task Contract Program to produce a controlled assignment;
- applying project contracts such as `AGENTS.md`, implementation requirements, plans, and accepted decisions;
- selecting relevant skills, artifacts, and prior evidence;
- invoking evaluators and using their evidence;
- applying an improvement strategy;
- deciding retries, branches, human escalation, and run completion.

The orchestrator submits completed Task Contracts through the Rekodo Runtime and receives only results that the runtime has already recorded. It is part of the Rekodo project while remaining outside the minimal runtime boundary.


The orchestration framework separates four areas of capabilities:

- [Task Contract Programs](docs/arch/task-contract-programs.md) — Produce structured, policy-controlled assignments for agents and tools.
- [Orchestration Patterns](docs/arch/orchestration-patterns.md) — Organize roles and tasks into sequential, iterative, or parallel work.
- [Improvement Strategies](docs/arch/improvement-strategy.md) — Decide what should follow a result, such as acceptance, critique, verification, revision, or escalation.
- [Evaluators](docs/arch/evaluators.md) — Supply evidence about correctness, quality, completion, cost, or latency.


### A Example Run

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

<<: [README](README.md)






