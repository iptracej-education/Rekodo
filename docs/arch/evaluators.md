# Evaluators

Evaluators are user-space components that examine a recorded result, artifact,
or run and return evidence about its correctness, quality, or completion. They
remain separate from orchestration patterns, which organize the work, and
improvement strategies, which decide how to respond to the evidence.

Examples include:

- unit and integration tests;
- compilers;
- static analysis;
- tool-based verification;
- deterministic answer checks;
- model judges;
- human judgment;
- application-specific checks and quality measures.

An evaluator receives a defined subject and evaluation criteria. It may return:

- pass or fail;
- a numeric score;
- detected errors or violations;
- supporting evidence;
- confidence or uncertainty;
- a human decision.

When an evaluator requires an agent, model, tool, or human, the Rekodo Orchestrator uses a Task Contract Program to define what must be evaluated, which evidence may be used, and what result structure must be returned. Rekodo records and relays the evaluation like any other assignment.

An evaluator reports evidence. It does not modify the evaluated result or directly choose what happens next. The orchestrator and its improvement strategy consume the evidence and decide whether to accept, revise, retry, escalate, or complete the work.

Operational measurements such as cost, latency, token usage, cache usage, retries, and failure rates are derived separately from the canonical history. The orchestration policy may consider these measurements together with evaluator results.

Rekodo records the evaluator, its version and configuration, the subject and criteria it received, and the evidence it returned. This keeps evaluations attributable and allows results from different runs or evaluators to be compared without changing the runtime.

In sum, evaluators examine outcomes and provide evidence. Improvement strategies decide what to do with that evidence. Rekodo records both.

<<: [README](../../README.md) | <: [BACK - Improvement Strategy](improvement-strategy.md) 