---
name: software-system-anatomy
description: "Reconstructs a project's architecture and data flow through a software-system anatomy model: product experience, source-folder/module mapping, visual architecture map, lifecycle, data/control flow, state, events, actions, extension points, resources, and failure recovery. Use when the user asks to quickly understand an unfamiliar codebase, open-source project, framework, agent, frontend/backend app, embedded system, plugin system, architecture document, or how this project works without getting lost in file-by-file summaries."
---

# Software System Anatomy

Use this skill to turn an unfamiliar project from "a pile of files" into a clear operating story.

The goal is not to summarize every module. The goal is to recover the project's main spine:

```text
How the system wakes up -> how data enters -> who dispatches work -> where state changes
-> which actions create effects -> how extensions attach -> how failures recover.
```

Default to Chinese unless the user asks otherwise.
## Core Stance

Analyze the project through a stable expectation:

```text
Complex software system
= product experience
+ required capabilities
+ lifecycle runtime
+ data/control/event flow
+ state model
+ action executor
+ resource boundary
+ extension mechanism
+ feedback/recovery loop
```

Do not begin with "this folder does X". Begin with "this system stays alive by doing X".

Also ask "what experience or capability forces this module to exist?" A module explanation is weak if it names the module but does not explain the user/system need behind it.

If available, read project orientation files first: `AGENTS.md`, `CLAUDE.md`, `FILETREE.md`, `README`, `docs/`, architecture notes, ADRs, and package/build entrypoints. Treat indexes as maps only; verify with real source before citing.
## Workflow

1. Define the system boundary.
   Identify what counts as inside the system, what external actors feed it, and what outputs or side effects it produces.

2. Bridge product experience to system capabilities.
   Identify the core experience, required capabilities, and which modules exist because of those capabilities. Use `references/product-bridge.md` for this step.

3. Find the awakening path.
   Locate entrypoints, bootstrapping, dependency wiring, configuration loading, registration, and start/listen/run phases.

4. Recover the main flows.
   Trace data flow, control flow, event flow, state flow, action flow, extension flow, and failure flow. Use `references/analysis-checklist.md` when the project is non-trivial.

5. Map modules to roles.
   Explain why each important module exists in the operating story: entry, runtime, dispatcher, state holder, adapter, action executor, resource owner, extension point, observer, or recovery mechanism.

6. Compare with the mother pattern.
   Show where the project matches the expected model and where it has a distinctive "spark": unusual runtime shape, clever extension mechanism, surprising state model, hidden scheduler, or domain-specific flow.

7. Produce a learning report.
   Use `references/report-template.md` for the default report. Keep it architecture-first, data-flow-first, and concrete.

## Output Contract

Every report must be readable in two layers:

1. **First screen: anatomy snapshot.** Give the product experience, "not X but Y" reframing, runtime/control owner, core data artery, state/event heart, extension surface, top risks, first reading target, and project spark.
2. **Evidence layer: expandable detail.** Put code references, longer flow explanations, edge cases, and uncertain inferences after the snapshot.

HTML reports must include a draggable/zoomable visual architecture canvas before long details. Use `references/architecture-canvas.md` to show nodes, relationships, vivid flow direction, and where each node lives in the repository.
When source files are available, include folder/module analysis. Use `references/source-structure.md` to explain what each important folder contains, which architecture module it maps to, why it exists, and where to start reading.

The first screen should answer:

```text
What is this thing?
What experience does it provide?
What keeps it alive?
What flows through it?
Who controls whom?
Where is state?
Where do outside abilities plug in?
What is special or risky?
Where should I read first?
```

## Reference Loading

Load only what the task needs:

- `references/core-model.md` - the conceptual model and vocabulary.
- `references/product-bridge.md` - product experience, capability-to-module derivation, technical choice tradeoffs, and modification entry points.
- `references/architecture-canvas.md` - visual architecture map and module-to-files contract for HTML/SVG/canvas outputs.
- `references/source-structure.md` - folder-to-module and key folder analysis for real repositories.
- `references/analysis-checklist.md` - questions for source exploration.
- `references/domain-mapping.md` - mappings for backend, frontend, embedded, agent, framework, game, and plugin systems.
- `references/report-template.md` - default output structure.

## Output Rules

Always prefer a small number of high-signal diagrams, tables, and source references over exhaustive prose.

When source files are available:

- Cite concrete files and functions/classes for each major claim.
- Mark uncertain inferences as "推测", say what evidence would confirm them, and separate observed facts from architecture interpretation.
- If source files are unavailable, state that the report is based on docs/user context and avoid inventing module names or flows.

## Default Report Sections

Use these sections unless the user asks for a different shape:

1. 产品体验与能力拆解
2. 30 秒总览
3. 架构预览图
4. 一句话主线
5. 核心动脉图
6. 母题映射表
7. 模块到文件夹/文件映射
8. 关键文件夹剖析
9. 数据流转链路
10. 控制权、状态与事件
11. 模块为什么必须存在
12. 扩展点、资源边界与失败恢复
13. 项目特殊火花
14. 建议阅读路线

## What Not To Do

- Do not output a flat directory tour unless the user explicitly asks for one.
- Do not force every project into event-driven, plugin-based, or state-machine language when the evidence is weak.
- Do not treat architecture as automatically good because it is abstract.
- Do not drown the user in minor functions before explaining the main operating story.
- Do not stay abstract; explain real folders/files and the capability pressure behind each module when source is available.
- Do not let cards, tables, Mermaid, or prose be the only overview; provide a visual node-link architecture map for HTML reports.
