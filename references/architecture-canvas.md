# Architecture Canvas

Use this reference when producing HTML or visual reports. The output must let a human see the architecture before reading paragraphs.

When a `design-style` folder exists, pick one style document from it and state which style is applied. However, the chosen style must not break the architecture readability rules in this file.

## Purpose

The architecture canvas is a visual node-link map:

```text
external inputs -> adapters -> runtime/control plane -> state/model -> action executors -> outputs
                                      |                         |
                                      v                         v
                              extension surface          resources/failure loop
```

It is not decoration. It is the architecture preview. The reader should understand the project shape before reading paragraphs.

## Rendering Strategy

Use this priority order:

1. **Inline SVG** for most architecture maps. This is the default for readable single-file HTML because text, arrows, labels, hover states, and layout are easier to control.
2. **Interactive HTML Canvas** only when the skill already has a fixed renderer or the graph is very large.
3. **Mermaid** only for quick Markdown fallback, not for final visual reports.

Do not let the model freestyle a complex Canvas renderer unless the renderer is already part of the skill. If Canvas is used, the same architecture data model, spacing, edge, and quality rules below still apply.

## Required Content

Every architecture canvas must include:

- **Nodes**: concrete project modules, runtimes, resources, and external actors.
- **Edges**: directional relationships between nodes.
- **Flow direction**: arrows must show where data/control/events move.
- **Legend**: explain edge colors/styles.
- **Where to find it**: each important architecture node must map to repository folders/files.
- **Evidence anchors**: each important node should have a file/module reference in a tooltip, side panel, or table below the canvas.
- **Interpretation line**: one sentence saying what the map reveals.

## Data Model First

For SVG/Canvas generation, first build a small data model:

```js
const architecture = {
  nodes: [
    {
      id: "external-audio",
      label: "Mic",
      layer: "External",
      role: "input",
      location: "hardware",
      evidence: "INMP441 / I2S input"
    },
    {
      id: "audio",
      label: "AudioService",
      layer: "Adapter",
      role: "capture / opus / playback",
      location: "main/audio/",
      evidence: "audio_service.*"
    },
    {
      id: "app",
      label: "Application",
      layer: "Runtime",
      role: "main event loop / scheduler",
      location: "main/",
      evidence: "application.*"
    }
  ],
  edges: [
    { from: "external-audio", to: "audio", type: "data", label: "PCM frames", main: true },
    { from: "audio", to: "app", type: "event", label: "wake / VAD" }
  ]
};
```

Render from this model. Do not handwave the graph in prose. Do not draw directly from vague module names.

## Node Layers

Arrange nodes in stable visual bands:

| Layer | Typical nodes |
| --- | --- |
| External world | user, browser, server, sensor, broker, model, filesystem, hardware |
| Entry/adapters | API routes, CLI, Board, Protocol, AudioService, UI event handlers |
| Runtime/control plane | Application, scheduler, event loop, framework container, orchestrator |
| State/model | state enum, store, DB, cache, context, actor state, queue |
| Action executors | DB writer, HTTP client, hardware driver, renderer, tool executor |
| Resources | locks, queues, pools, buffers, I2C/I2S, transactions, permissions |
| Extension surface | plugin, provider, hook, tool, adapter, board implementation |
| Outputs/feedback | UI, speaker, actuator, network response, logs, retry, alert, reboot |

The runtime/control plane must be visually central and larger than normal nodes.

## Layout and Spacing Rules

Readable spacing is more important than filling the screen.

### Default SVG canvas

Use a wide viewBox for project architecture diagrams:

```html
<svg viewBox="0 0 3400 1900" role="img" aria-label="Architecture map">
```

### Recommended horizontal columns

```text
x=120    External inputs
x=660    Entry/adapters
x=1220   Runtime/control plane and cloud roundtrip
x=1840   State/model and extension surface
x=2380   Action executors and recovery
x=2940   Outputs/feedback
```

### Minimum spacing

- Normal node width: 260-320 px.
- Important runtime node width: 360-440 px.
- Horizontal gap between columns: at least 300 px, preferably 450-600 px.
- Vertical gap between stacked nodes: at least 160 px, preferably 220 px.
- Main artery should have a clear lane; do not run unrelated lines through it.
- Resource and failure links should be pushed to the bottom or right-bottom area.

### If the diagram feels crowded

Do not shrink everything. Instead:

1. Increase the SVG viewBox width/height.
2. Group low-priority nodes into clusters.
3. Remove non-essential edges.
4. Route edges around the outside of node groups.
5. Put explanatory details into tooltip/table, not into the node.

## Edge Types

Use visual encoding consistently:

| Edge type | Meaning | Required style |
| --- | --- | --- |
| Data flow | payload/data/audio/request moves | vivid blue `#0077c8`, solid, 4px; main artery 5-6px |
| Control flow | runtime calls or schedules code | purple `#6d28d9`, solid, 3.5-4px |
| Event/state flow | event triggers state transition | amber `#d97706`, dashed, 3.5-4px |
| Extension link | plugin/tool/adapter attaches to core | green `#15803d`, solid, 3.5-4px |
| Resource ownership | module owns/protects shared resource | slate `#475569`, dotted, 2.5-3px |
| Failure/recovery | error, retry, fallback, alert | red `#dc2626`, dashed, 3.5-4px |

## Edge Visibility Rules

The graph must make paths visually separable at a glance:

- Use saturated, high-contrast edge colors, but avoid turning the map into “all lines”.
- Arrowheads must use the same color as the edge.
- Arrowheads should be readable but not oversized. Recommended marker size: `8-10px`; avoid huge arrowheads that dominate nodes.
- Main data artery must be the strongest path: 5-6px line, solid, visible arrowheads, optional subtle glow.
- Secondary edges should usually be 3-4px.
- Resource dotted edges should be quieter: 2.5-3px.
- Dashed edges need long dashes (`12px`) with clear gaps (`8px`).
- Edge labels should sit on a small background chip so they do not disappear on crossing lines.
- Do not label every edge. Label only edges that clarify the architecture.
- On hover or selection, increase the selected edge width and dim unrelated edges.

## Arrowhead Rules

Bad architecture maps often fail because the arrowheads are too large.

For SVG, prefer:

```html
<marker markerWidth="10" markerHeight="10" refX="8" refY="5" orient="auto" markerUnits="strokeWidth">
  <path d="M 0 0 L 10 5 L 0 10 z" />
</marker>
```

For very thick main data edges, use a separate marker no larger than `12x12`.

Avoid arrowheads larger than the node text height.

## Edge Routing Rules

Avoid hairball graphs by giving edge categories their own lanes:

- **Main data lane**: upper-middle or center lane.
- **Control lane**: from runtime to state/executors, mostly horizontal.
- **Event/state lane**: from adapters/runtime to state, often dashed and slightly offset.
- **Resource lane**: bottom lane, dotted.
- **Failure/recovery lane**: bottom-right lane, red dashed.
- **Extension lane**: bottom-middle or right-middle lane, green.

Prefer curved or elbow paths that leave from node sides. Do not run lines through node bodies.

## Node Count Budget

Avoid hairball graphs. If there are more than 18 nodes, group nodes into clusters.

Recommended budget:

- External: 2-3 nodes
- Adapters: 3-4 nodes
- Runtime: 1 main node
- State/model: 1-2 nodes
- Executors: 2-4 nodes
- Outputs: 3-5 nodes
- Resources/recovery/extension: 2-4 nodes

If a project has many classes, compress them into architecture-level nodes and preserve file evidence in tooltip/table.

## Module-to-Files Map

The visual graph must be paired with a repository map unless the user explicitly asks for only the diagram.

Use this table below the canvas or in a side panel:

| Architecture node | Repository location | Key files/classes | Role in the graph | Start reading here |
| --- | --- | --- | --- | --- |
| Runtime/control plane |  |  |  |  |
| Entry/adapters |  |  |  |  |
| State/model |  |  |  |  |
| Action executors |  |  |  |  |
| Resource owners |  |  |  |  |
| Extension surface |  |  |  |  |
| Failure/recovery |  |  |  |  |

Rules:

- Map conceptual nodes to real directories/files, not just module names.
- If a node spans several files, list the main folder plus 2-4 key files.
- If a node is inferred from scattered code, say `scattered` and name the strongest evidence.
- Use the same node labels in the canvas and the map table.
- The “Start reading here” column should name the first function/class/file that reveals the node’s responsibility.

## Visual Report Shape

For full HTML reports, use this order:

1. Hero verdict: “not X, but Y”, product experience, one-sentence spine.
2. Snapshot cards: experience, runtime/control owner, data artery, state/event heart, extension surface, risk boundary, read-first, spark.
3. Architecture canvas: node-link map with legend and highlighted main artery.
4. Module/folder map: same node labels mapped to repository folders/files.
5. Evidence layer: collapsible file/function references.

If the user asks “只画架构图”, output only the architecture-map section, legend, and short interpretation line.

## Inline SVG Minimum

If using inline SVG, include:

```html
<section class="architecture-map">
  <div class="map-header">
    <h2>架构预览图</h2>
    <div class="map-toolbar" aria-label="SVG controls">
      <button type="button" data-action="zoom-in">+</button>
      <button type="button" data-action="zoom-out">-</button>
      <button type="button" data-action="fit">Fit</button>
      <button type="button" data-action="expand">Expand</button>
    </div>
  </div>
  <div class="canvas-frame">
    <svg viewBox="0 0 3400 1900" role="img" aria-label="Architecture node-link map">
      <g id="viewport">...</g>
    </svg>
  </div>
  <div class="legend">...</div>
  <p class="map-reading">这张图说明：...</p>
</section>
```

SVG interaction requirements:

- Drag inside the SVG to pan.
- Mouse wheel zooms around the pointer when practical.
- `+` and `-` buttons zoom in/out.
- `Fit` resets the graph to a readable full-map view.
- `Expand` enlarges the map frame or enters fullscreen-style view.
- Use `cursor: grab` and `cursor: grabbing` while dragging.

## HTML Canvas Minimum

If using `<canvas>`, include:

```html
<section class="architecture-map">
  <div class="map-header">
    <h2>架构预览图</h2>
    <div class="map-toolbar" aria-label="Canvas controls">
      <button type="button" data-action="zoom-in">+</button>
      <button type="button" data-action="zoom-out">-</button>
      <button type="button" data-action="fit">Fit</button>
      <button type="button" data-action="expand">Expand</button>
    </div>
  </div>
  <div class="canvas-frame">
    <canvas id="architectureCanvas" width="3400" height="1900" aria-label="Draggable architecture node-link map"></canvas>
  </div>
  <div class="legend">...</div>
  <p class="map-reading">这张图说明：...</p>
</section>
```

Canvas interaction requirements:

- Drag inside the canvas to pan.
- Mouse wheel or trackpad pinch zooms around the pointer.
- `+` and `-` buttons zoom in/out.
- `Fit` resets the graph to a readable full-map view.
- `Expand` enlarges the canvas frame or enters fullscreen-style view.
- The toolbar sits at the top-right of the canvas card.
- The canvas frame has a fixed min-height and `overflow: hidden`; the graph moves inside it, not by making the whole page huge.
- Use `cursor: grab` and `cursor: grabbing` while dragging.

The script must draw:

- rounded nodes with labels
- directional arrows
- edge labels when they clarify the flow
- colored edge types from the legend
- highlighted main data artery
- at least one visible resource/risk boundary
- hover/selected edge highlighting when practical

## Visual Quality Rules

- The runtime/control plane should be visually central.
- Main data flow should be the strongest visual path, but it should not bury the nodes.
- Secondary flows should be thinner, dashed, or routed through separate lanes.
- Lines must be readable, but not so dominant that the whole graph becomes “all arrows”.
- Avoid hairball graphs. If there are more than 18 nodes, group nodes into clusters.
- Use labels short enough to fit inside nodes.
- Do not use color decoratively; color must mean edge or node type.
- The graph should fit desktop width and degrade to horizontal scrolling or zoom on small screens.
- If the map looks crowded, expand the coordinate space before reducing font sizes.

## Architecture Map Test

Before finishing, ask:

- Can the user identify the central runtime/control owner without reading text?
- Can the user trace the main data flow with their eyes?
- Can the user see which modules are adapters vs action executors?
- Can the user use the map table to know where each node lives in the repository?
- Can the user see extension points and resource/risk boundaries?
- Are node gaps large enough that edges do not swallow the diagram?
- Are arrowheads smaller than node text height and not visually overwhelming?
- Can the user name the project spark after looking at the map?

If any answer is no, simplify, spread out, or redraw the canvas.
