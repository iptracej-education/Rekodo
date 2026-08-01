# Task Contract Programs

For every worker assignment, the Rekodo Orchestrator runs a selected Task Contract Program.

A Task Contract Program is a programmable user-space process that turns an orchestration decision into a structured, validated Task Contract. It may combine ordinary program logic, policy rules, LLM reasoning, recorded evidence, and selected skills. The program controls the process; the LLM is an optional reasoning component inside it.

## Purpose

A Task Contract defines exactly what a worker has been assigned, what authoritative information governs the assignment, what the worker may access or decide, and what it must return.

The worker is not expected to reconstruct its assignment from conversation history, search the project for current rules, or remember agreements from an earlier context window. The Task Contract provides the current controlled assignment directly.

## Core Component 
```
Current assignment and recipient
Project contract and accepted decisions
Declared worker capabilities
Available skills
Pattern and strategy state
Access to recorded history
        │
        ▼
┌─────────────────────────────────┐
│     Task Contract Program       │
│                                 │
│  • program logic                │
│  • policy rules                 │
│  • optional LLM reasoning       │
│  • log queries                  │
│  • skill and evidence selection |          │
│  • structured validation        │
└──────────────┬──────────────────┘
               ├── Validation Record
               ▼
      Validated Task Contract
               │
               ▼
      Rekodo records and relays
```
The Task Contract Program can be implemented in any user-space language. It may inspect project documents, query recorded events, call an LLM, select skills, and apply task-specific orchestration logic.

## Task Contract Contents

A Task Contract should minimally define:
```
contract identity and version
task and recipient
objective
authoritative inputs
requirements and constraints
permissions and worker capabilities
selected skills and evidence
completion checks
required result structure
escalation conditions
causal relationship to prior events
```
The final contract is immutable. A change creates a new contract version rather than silently modifying the existing assignment.

A Task Contract is not necessarily an LLM prompt. After validation, a worker-specific renderer may convert it into a prompt, JSON request, command, tool arguments, or another backend format.

## LLM Responsibilities

An LLM may assist with semantic work that ordinary rules cannot perform reliably, such as:

- decomposing a task within the approved objective;
- identifying relevant prior decisions and evidence;
- selecting useful skills from the available set;
- explaining the assignment to the worker;
- identifying risks, conflicts, and ambiguities;
- proposing implementation steps;
- determining which upstream results matter;
- proposing additional completion checks;
- identifying when human clarification is needed.

The LLM produces a structured proposal. It does not produce an executable assignment without programmatic validation.

LLM reasoning is optional. A Task Contract Program may construct a contract entirely through ordinary code when semantic reasoning is unnecessary.

## Program and Policy Responsibilities

Programmatic policy owns or constrains:

- the governing objective and non-negotiable requirements;
- project invariants and contract precedence;
- the recipient and declared worker capabilities;
- authoritative document and artifact versions;
- readable and writable resources;
- allowed tools and network access;
- dependency changes;
- token, cost, and time limits;
- mandatory tests and checks;
- the required result schema;
- escalation conditions;
- whether the final contract is valid.

The LLM may propose values only for fields opened to it by the program. It
cannot remove mandatory requirements, widen permissions, replace authoritative
inputs, or weaken completion criteria.


## Expected Flow 

1. The orchestration pattern selects the next assignment and recipient.
2. The Task Contract Program loads authoritative inputs and current policy.
3. The program fixes fields that the LLM may not change.
4. When semantic reasoning is needed, the program requests a structured LLM
proposal.
5. The program selects relevant evidence and skills and merges the proposal
without overriding locked fields.
6. The policy validator checks the resulting contract for completeness and
authorization.
7. The program records a validation result describing violations, repairs, and
policy decisions.
8. Invalid contracts are repaired, retried, or escalated rather than delivered.
9. Rekodo records the accepted Task Contract.
10. Rekodo relays the recorded contract to the declared worker.

Conceptually:

```python
def produce_task_contract(state, worker, policy):
    locked = policy.required_contract_fields(state, worker)

    proposal = ask_llm_for_structured_proposal(
        task=locked.objective,
        project_contract=locked.project_contract,
        run_state=state,
        worker=worker,
    )

    contract = merge_without_overriding_locked_fields(
        locked=locked,
        proposal=proposal,
    )

    policy.validate(contract)
    return contract
``` 

## Recording LLM-Assisted Generation
Any LLM request or response that influences a Task Contract is part of the
recorded run.

```
Task Contract Program requires semantic reasoning
            ↓
submits a structured reasoning request through Rekodo
            ↓
Rekodo records and relays the request
            ↓
LLM returns a structured proposal
            ↓
Rekodo records and relays the proposal
            ↓
Task Contract Program merges and validates it
            ↓
validation result is recorded
            ↓
Rekodo records and relays the accepted Task Contract
```
The LLM request used during contract generation is a fixed, policy-bounded internal orchestration operation. It does not recursively require another Task Contract Program.

Rekodo records:

- the Task Contract Program version;
- the schema and policy versions;
- the exact authoritative inputs or their content hashes;
- the LLM request and response, when used;
- the validation result and any repairs;
- the final accepted Task Contract.

This makes it possible to determine:

- which program and policy produced the contract;
- which information was available;
- which LLM contributed reasoning;
- what the LLM proposed;
- what policy rejected or changed;
- why the final contract differed from the proposal;
- which exact contract governed the worker.

The orchestration program remains the controller even when an LLM contributes reasoning.

<<:  [README](../../README.md) | [NEXT - Orchestration Patterns](orchestration-patterns.md)>:  