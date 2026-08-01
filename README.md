# Rekodo 

Minimal, improvable orchestration for multi-agent runs. Every run recorded and the next run improved.

Rekodo combines a minimal runtime core with an extensible user-space orchestration framework. 

> [!NOTE]
> Rekodo is under active development. Execution code will be published with the first alpha release. 

## Core Foundation

Rekodo is a framework for coordinating multi-agent work while preserving what happened during each run. It records the user’s input, every assignment, and every returned result in one durable, ordered history, so the work can be understood and resumed even after context compression or restart.

Rekodo separates two jobs. A small runtime records and passes each exchange, while a separate orchestration layer decides which agent or tool should act, what it should receive, and what should happen next. This keeps the runtime stable while allowing each project to customize how work is planned, checked, and improved.

Rekodo improves work from recorded results and evaluations. During a run, an improvement strategy may request critique, verification, revision, retry, or human review. After a run, accepted findings may be carried forward as memory, skills, policies, or better orchestration for later runs.

See [Foundation](docs/arch/foundation.md) for the complete design principles.

## Architecture

Rekodo has two architectural layers: the **Rekodo Runtime** and the **Rekodo Orchestrator**. 

![Recode Architecture](./docs/arch/diagram.png)
(By GPT-5.6-sol) 

### Rekodo Runtime

TThe Rekodo Runtime remains deliberately unintelligent. It understands only generic execution information and is responsible for:

- recording an input before making it available to orchestration;
- recording a Task Contract before delivering it to its declared worker;
- recording a result or evaluation before relaying it to orchestration;
- assigning a canonical order to recorded events;
- preserving and exposing the canonical history.

The runtime does not select tasks or workers, interpret project contracts, construct worker assignments, evaluate results, or decide what happens next. It records, orders, and relays the execution chosen by user-space orchestration.

After context compression or process restart, the orchestrator can reconstruct what has already happened from the canonical history instead of depending on an agent's retained conversation state.

### Rekodo Orchestrator

Rekodo provides a default orchestrator as a separate, replaceable user-space component. Applications may extend it or bring their own orchestrator as long as they communicate through the Rekodo Runtime protocol.

The orchestrator is a programmable framework, not an LLM. It may combine ordinary program logic with commercial, open-source, or local LLMs as reasoning components.

The orchestrator controls the run. It is responsible for:

- selecting the next task and its recipient;
- applying an orchestration pattern;
- running a Task Contract Program for each worker assignment;
- applying project contracts such as `AGENTS.md`, implementation requirements, plans, and accepted decisions;
- selecting relevant skills, artifacts, and prior evidence;
- querying recorded events and derived views of previous work;
- invoking evaluators and consuming their evidence;
- applying an improvement strategy;
- deciding retries, branches, human escalation, and run completion.

A **Task Contract Program** is not the orchestrator. It is a programmable component used by the orchestrator to produce one structured, policy-controlled Task Contract for one worker assignment.

The orchestrator submits completed Task Contracts through the Rekodo Runtime and receives only results that the runtime has already recorded. It remains outside the minimal runtime boundary.


### Orchestration Capabilities

The user-space orchestration framework separates four concerns:

- [Task Contract Programs](docs/arch/task-contract-programs.md) — Produce structured, policy-controlled assignments for agents and tools.
- [Orchestration Patterns](docs/arch/orchestration-patterns.md) — Organize roles and tasks into sequential, iterative, or parallel work.
- [Improvement Strategies](docs/arch/improvement-strategy.md) — Propose what should follow a result, such as acceptance, critique, verification, revision, retry, or escalation.
- [Evaluators](docs/arch/evaluators.md) — Supply evidence about correctness, quality, or completion.

Patterns organize the work. Task Contract Programs define controlled assignments. Improvement strategies propose what follows an outcome. Evaluators supply the evidence. Queries derive operational measurements from the recorded history. The runtime records and relays the execution.

### An Example Run

```text
User submits a task through a client
        ↓
Runtime records the input
        ↓
Orchestrator selects the next task and worker
        ↓
Task Contract Program produces a controlled assignment
        ↓
Runtime records and delivers the Task Contract
        ↓
Worker (agent or tool) performs the assignment and returns a result
        ↓
Runtime records and relays the result
        ↓
Evaluator supplies evidence
        ↓
Runtime records and relays the evaluation
        ↓
Improvement strategy proposes what should happen next
        ↓
Orchestrator accepts, revises, retries, escalates, or completes
        ↓
Runtime records the next action or completion
```

The orchestrator decides what happens next. The runtime records and relays each execution exchange.








