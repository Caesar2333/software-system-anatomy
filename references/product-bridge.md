# Product Bridge

Use this reference to connect product experience to system anatomy. The goal is not a marketing analysis; it is to explain why the architecture had to grow these parts.

## Core Move

Before mapping lifecycle, state, events, and extension points, answer:

```text
What experience or system capability makes these modules necessary?
```

A good explanation moves like this:

```text
surface category
  -> actual experience
  -> required capabilities
  -> modules that provide those capabilities
  -> flows that connect the modules
  -> risks where the capabilities share resources or state
```

## Product Experience Reframe

Use the pattern:

```text
This project looks like [surface category], but the real experience is [actual experience].
Therefore it must provide [capability A], [capability B], [capability C].
Those capabilities explain why [module X], [module Y], and [module Z] exist.
```

Examples:

```text
It looks like an ESP32 firmware project, but the real experience is a desktop robot that can listen, speak, show expressions, move, sense gestures, and play music.
```

```text
It looks like an RPC framework, but the real experience is letting developers call remote services as if they were local methods.
```

## Capability Table

Use this when a report feels too abstract:

| Product/system capability | Why it is needed | Module(s) that provide it | What breaks without it |
| --- | --- | --- | --- |
| Input | How users or the outside world send information in |  |  |
| Runtime/control | How the project decides what happens next |  |  |
| State/memory | How the project remembers mode, progress, or context |  |  |
| Output/action | How the project affects users, devices, files, networks, or other systems |  |  |
| Communication | How it connects with external systems |  |  |
| Extension | How it grows without rewriting the core |  |  |
| Recovery | How it keeps working under failure or pressure |  |  |

## Module Necessity Table

Use this to make module explanations sharper:

| Capability pressure | Module | Why this module must exist | Design cost / risk |
| --- | --- | --- | --- |
|  |  |  |  |

The "design cost / risk" column matters. Every important module solves a problem but adds complexity: extra state, resource contention, async boundaries, configuration, lifecycle hooks, or testing burden.

## Technical Choice Check

For each important technical choice, explain:

```text
The project chooses [X] because [constraint].
Without [X], [failure or limitation] would happen.
The cost is [complexity], so the system needs [guardrail].
```

Use this sparingly. Only include choices that clarify the main architecture.

## Modification Entry Points

When the user may customize or fork the project, end with a compact table:

| Modification goal | Where to inspect/change | Main risk |
| --- | --- | --- |
| Add a new input |  | event/state/resource interaction |
| Add a new output/action |  | side effects, concurrency, resource ownership |
| Add a new protocol/provider |  | lifecycle and interface contract |
| Add a new plugin/tool |  | permissions, schema, error handling |
| Change state behavior |  | illegal transitions, stale UI, recovery paths |

Do not make modification advice the center unless the user asks. Keep it as a bridge from understanding to action.

## Guardrail

Do not let product perspective become vague product prose. Every product claim must land on modules, flows, or risks.

