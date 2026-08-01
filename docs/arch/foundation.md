## Foundation

Many agent systems combine the machinery that executes a run with the logic that plans, remembers, evaluates, and improves it. Rekodo separates these responsibilities: a minimal runtime records and relays what happens, while user-space orchestration decides what should happen next.

Rekodo records every input, Task Contract, action, and result that passes through it in one canonical, ordered history. This gives agents and humans the same account of what was assigned, what information was available, and what returned—even after context compression, process restart, or handoff.

Rekodo records every input, Task Contract, action, and result that passes through it in one canonical, ordered history. This gives agents and humans the same account of what was assigned, what information was available, and what returned—even after context compression, process restart, or handoff. User-space queries derive measurements from that history, while evaluators provide evidence about task outcomes.

The runtime core is intentionally unintelligent. It records, orders, relays, and replays execution; it does not plan work, select workers, interpret project contracts, evaluate results, or decide how the system should improve. Those responsibilities live in the user-space orchestration framework, where Task Contract Programs, orchestration patterns, evaluators, memory, skills, and improvement strategies can evolve without embedding their logic in the runtime.

Rekodo carries findings across runs explicitly rather than learning silently. A finding affects a future run only when user-space policy accepts it and includes it as a versioned input, such as an updated project contract, orchestration pattern, memory, skill, or improvement strategy. Its provenance remains connected to the events and evaluations that produced it.

Within a run, coordination occurs through explicit, recorded assignments, messages, and handoffs rather than hidden shared state. The canonical history can be replayed, queried, compared, and forked, while graphs, reports, state views, and datasets remain derived user-space representations. This keeps the runtime stable while allowing orchestration above it to remain programmable and improvable.

<<: [[README](../../README.md)]