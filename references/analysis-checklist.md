# Analysis Checklist

Use this checklist while exploring source. Do not mechanically answer every question in the final report; use it to find the main story.

## 1. Boundary

- What is the project trying to be: app, framework, library, runtime, agent, service, device firmware, CLI, plugin host?
- Who are the external actors: user, browser, server, sensor, message broker, model, tool, filesystem, database, plugin author?
- What enters and leaves the system?
- What is explicitly outside the system?

## 2. Awakening Path

- Which file or command starts the system?
- What configuration is loaded first?
- What core context/container/runtime is created?
- What components register themselves?
- What begins listening, polling, rendering, scheduling, or looping?
- Where is shutdown or cleanup handled?

## 3. Data Flow

- What are the main data shapes?
- Where does raw input become normalized internal data?
- Where does data branch into multiple paths?
- Where is data persisted, cached, transformed, enriched, filtered, serialized, or sent out?
- What code changes the meaning of the data, not just its format?

## 4. Control Flow

- Is the project controlled by `main`, by a framework, by an event loop, by a scheduler, by a runtime, or by an agent loop?
- Which module is the dispatcher/orchestrator?
- Where does control invert: framework calling user code, plugin host calling plugin, runtime calling handler?
- Where are decisions made?

## 5. Event Flow

- What counts as an event in this project?
- Who produces events?
- Who routes or broadcasts them?
- Who listens or subscribes?
- Are events durable messages, in-memory notifications, callbacks, interrupts, UI events, or model/tool observations?
- Are event names and payloads explicit enough to trace?

## 6. State Flow

- What are the core states?
- Where are they stored: enum, class field, store, context, DB, cache, queue, actor mailbox, firmware variable?
- Which events trigger state transitions?
- Which transitions are legal, guarded, retried, or forbidden?
- Which modules react when state changes?

## 7. Action Flow

- Where are side effects produced?
- Which actions touch external systems: DB, network, filesystem, hardware, browser APIs, model/tool calls?
- Are actions synchronous, async, queued, transactional, idempotent, cancelable, or retryable?
- What protects actions from invalid state?

## 8. Resource Boundary

- What resources can be contested or exhausted?
- Are there locks, queues, pools, semaphores, transactions, rate limits, buffers, leases, or permissions?
- Which module owns each resource?
- What happens under pressure?

## 9. Extension Flow

- Where can new behavior attach?
- What is the extension vocabulary: plugin, provider, adapter, middleware, hook, starter, strategy, handler, skill, tool?
- When are extensions discovered and registered?
- What contract must an extension satisfy?
- Can extensions affect lifecycle, data flow, event flow, state, or actions?

## 10. Failure Flow

- Where are errors caught?
- Which failures retry, degrade, rollback, fallback, reconnect, restart, or surface to users?
- Is there a watchdog, supervisor, circuit breaker, timeout, health check, or dead-letter path?
- How do logs, tests, metrics, traces, or UI feedback close the loop?

## 11. Reading Route

Build a route for the user:

1. Entry/start file
2. Runtime/context/container setup
3. Dispatcher/router/scheduler
4. Core state model
5. Main data/event handlers
6. Side-effect/action modules
7. Extension/adaptation layer
8. Failure/recovery code

