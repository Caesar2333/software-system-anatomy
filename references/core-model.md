# Core Model

Use this vocabulary to analyze a project as a living software system rather than a static file tree.

## Mother Pattern

```text
Software system
= bounded runtime
+ lifecycle
+ input/data flow
+ control flow
+ event flow
+ state model
+ action execution
+ resource ownership
+ extension mechanism
+ failure feedback loop
```

The working assumption is not "every project is secretly the same." The assumption is:

```text
Most long-running systems must answer the same engineering questions.
Each project answers them with its own names, files, conventions, and tradeoffs.
```

## Key Questions

- What wakes the system up?
- What keeps it running?
- What data enters the system?
- Who owns the main control flow?
- What state does the system remember?
- What events or inputs cause state changes?
- Where are real-world effects produced?
- Which resources are protected or shared?
- Where can outside code plug in?
- How does the system notice, report, retry, degrade, or recover from failure?
- Where does each architecture node live in the repository?
- What does each important folder own, and why does it exist?

## Core Vocabulary

| Term | Meaning | Evidence to look for |
| --- | --- | --- |
| Boundary | What is inside the system and what is outside | public API, CLI, HTTP endpoints, device inputs, UI boundary, tool boundary |
| Lifecycle | The system's route from birth to shutdown | init/start/run/stop hooks, framework boot phases, setup/teardown |
| Main loop | The repeated structure that keeps work happening | event loop, server listen loop, scheduler, task loop, render loop, agent loop |
| Data flow | How information moves and changes | DTOs, requests, commands, messages, stores, transformations |
| Control flow | Who decides the next step | main function, framework runtime, scheduler, orchestrator, router |
| Event flow | How "something happened" is represented and distributed | events, callbacks, listeners, queues, pub/sub, interrupts |
| State model | What situation the system thinks it is in | enums, stores, contexts, DB rows, caches, session, actor state |
| Action executor | Code that creates side effects | DB writes, network calls, filesystem, hardware IO, model/tool calls |
| Resource boundary | Scarce or shared things needing protection | locks, pools, buffers, transactions, rate limits, permissions |
| Extension point | Where new behavior attaches without rewriting the core | plugins, hooks, providers, adapters, middleware, starters |
| Feedback loop | How outputs/errors return to guide next action | logs, tests, traces, retries, watchdogs, supervisors, health checks |

## The Spine

The architecture report should recover a spine like this:

```text
External actor/input
  -> entrypoint
  -> validation/normalization
  -> dispatcher/runtime
  -> state lookup/update
  -> action execution
  -> output/side effect
  -> feedback/recovery
```

Not every project has every step. Missing steps are useful observations.

## Project Spark

After mapping the project to the mother pattern, identify what is special:

- Does it hide the main loop inside a framework?
- Does it treat data as events, commands, documents, frames, packets, or tasks?
- Does it have a surprising state model?
- Does it use plugins, adapters, or providers in a distinctive way?
- Does it solve failure, pressure, or resource sharing elegantly?
- Does it avoid complexity by staying direct and simple?

This "spark" is often the part the user will remember.

## Guardrails

- Do not force every project into event-driven, plugin-based, or state-machine language when the evidence is weak.
- Do not call a callback an architecture by itself.
- Do not assume async, queues, plugins, IoC, or abstractions are automatically better.
- Keep observed facts separate from interpretation.
- Prefer direct calls for small systems; introduce heavier concepts only when state, change, concurrency, failure, or extension pressure justifies them.
