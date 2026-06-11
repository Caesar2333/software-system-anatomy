# Domain Mapping

Use this file to translate the mother pattern into the project's local domain. Do not force terminology; prefer the names used by the codebase.

## Backend Service

| Mother pattern | Common backend form |
| --- | --- |
| Boundary | HTTP API, RPC, message consumer, CLI |
| Lifecycle | app bootstrap, DI container, server listen, graceful shutdown |
| Main loop | web server event loop, worker loop, queue consumer |
| Data flow | request -> DTO -> domain command/query -> DB/message/response |
| Control flow | router, controller, service layer, middleware chain |
| Event flow | domain events, queue messages, pub/sub, hooks |
| State model | DB rows, sessions, caches, aggregates, workflow states |
| Action executor | repositories, clients, jobs, mailers, storage adapters |
| Extension point | middleware, interceptors, providers, modules, plugins |
| Failure flow | validation errors, retries, transactions, DLQ, circuit breaker |

## Frontend App

| Mother pattern | Common frontend form |
| --- | --- |
| Boundary | browser UI, route, API client, local storage |
| Lifecycle | app mount, route transitions, component mount/update/unmount |
| Main loop | browser event loop, render loop, framework reconciliation |
| Data flow | user input/API response -> state/store -> render/output |
| Control flow | router, framework runtime, event handlers, effects |
| Event flow | DOM events, component events, store subscriptions, websocket messages |
| State model | component state, global store, query cache, URL state |
| Action executor | fetch calls, DOM updates, storage writes, analytics |
| Extension point | hooks, providers, plugins, slots, route modules |
| Failure flow | error boundaries, retry UI, fallback states, validation feedback |

## Embedded / RTOS

| Mother pattern | Common embedded form |
| --- | --- |
| Boundary | sensors, buttons, network, display, actuators |
| Lifecycle | bootloader, board init, driver init, task creation, shutdown/sleep |
| Main loop | RTOS scheduler, task loops, ISR-triggered work |
| Data flow | sensor frame/event -> queue/buffer -> task -> driver/protocol |
| Control flow | scheduler, task priorities, event groups, callbacks |
| Event flow | interrupts, queues, timers, event groups, protocol messages |
| State model | device state enum, flags, NVS, connection state |
| Action executor | I2C/I2S/SPI/UART, Wi-Fi, display, motor, audio |
| Extension point | drivers, board abstraction, protocol adapters |
| Failure flow | watchdog, reconnect, timeout, fallback mode, reset |

## Agent / Tool-Using AI System

| Mother pattern | Common agent form |
| --- | --- |
| Boundary | user request, workspace, tools, model, permissions |
| Lifecycle | observe -> plan -> act -> verify -> revise -> finish |
| Main loop | agent loop, planner/executor loop, task queue |
| Data flow | user goal -> plan -> tool calls -> observations -> answer/artifact |
| Control flow | orchestrator, planner, policy, tool router |
| Event flow | tool result, test failure, permission denial, file change, user feedback |
| State model | conversation context, plan state, workspace diff, memory |
| Action executor | file edits, shell commands, browser/tool/API calls |
| Extension point | skills, tools, MCP servers, plugins, connectors |
| Failure flow | diagnostics, retries, rollback proposals, clarification, handoff |

## Framework / Plugin Host

| Mother pattern | Common framework form |
| --- | --- |
| Boundary | user code, plugins, config, runtime environment |
| Lifecycle | create context -> discover/register -> initialize -> run -> dispose |
| Main loop | runtime-owned scheduler/event loop |
| Data flow | framework input -> normalized context -> user/plugin handlers |
| Control flow | inversion of control: framework calls user code |
| Event flow | lifecycle events, hooks, observers, middleware |
| State model | application context, registry, container, plugin state |
| Action executor | framework services and user handlers |
| Extension point | plugins, modules, providers, middleware, annotations, starters |
| Failure flow | startup validation, isolated plugin errors, cleanup hooks |

## Game / Simulation

| Mother pattern | Common game form |
| --- | --- |
| Boundary | player input, assets, physics, renderer, network |
| Lifecycle | load assets -> init world -> update/render loop -> pause/stop |
| Main loop | update tick, render frame, physics step |
| Data flow | input/network/time -> world state -> simulation -> render/audio |
| Control flow | engine loop, scene manager, systems |
| Event flow | input events, collision events, timers, scripted triggers |
| State model | game state, scene state, entity/component state |
| Action executor | physics, rendering, audio, save/load, network replication |
| Extension point | scripts, components, systems, mods |
| Failure flow | fallback assets, reconnect, state rollback, crash reports |

