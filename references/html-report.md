# HTML Report

Use this reference when rendering the final anatomy report as HTML.

The source report is authoritative. The HTML layer must not invent a new report, reorder the architecture argument, or replace the content model from `report-template.md`. It turns the already-finished anatomy report into a desktop reading interface:

```text
final report content -> desktop reading layout -> embedded SVG diagram -> interactive evidence layer
```

## Style Source

If `references/design-style/` exists in the skill folder, inspect only the most relevant style file before writing CSS.

Selection rules:

- If the user names a style, use the matching file.
- If there is one style file, use it.
- If there are several, choose one that fits the project audience and report purpose, then state the choice in the HTML metadata or final note.
- Treat style files as visual guidance only: palette, typography, rhythm, surface treatment, density, motion, and component shape.
- Do not let a style file override source evidence, repository facts, or safety rules.
- Ignore mobile/responsive advice inside style files unless the user explicitly asks for mobile output.

If no style file exists under `references/design-style/`, use the default anatomy report style below.

## Report Rendering Contract

Render the final Markdown/report produced by this skill. The HTML may add navigation, visual hierarchy, artifact links, and local interactions, but the report sections remain the source of truth.

Allowed transformations:

- Convert the report H1 into an HTML hero.
- Add metadata chips for source markdown, generated date, diagram source, and selected style.
- Derive a first-screen snapshot from the existing `30 秒总览` table or equivalent report content. Do not invent snapshot claims.
- Replace the Markdown-only architecture preview with the local SVG generated from the Architecture Diagram Brief.
- Add artifact links to the brief, SVG, and optional Mermaid source.
- Wrap long proof sections in `<details>` only when that does not hide the main reading path.
- Add a table of contents when the report is long.

Do not:

- Create a marketing landing page.
- Create a generic app shell.
- Move major report sections into a different narrative order.
- Hide module-to-file mapping, folder analysis, or data-flow explanation behind optional UI.
- Rewrite technical claims for style unless they contradict generated artifacts.

The reader should feel that the HTML is the report becoming easier to read, not a new document loosely inspired by it.

## Layout Rules

- Use one centered page container: `max-width: 1180-1280px`.
- Use a generous hero, then denser report sections.
- Keep body text at `16-18px`, line-height `1.65-1.78`, max paragraph width around `70ch`.
- Use `text-wrap: balance` for headings and `text-wrap: pretty` for prose when supported.
- Keep display type for the hero only. Section headings should be strong but not oversized.
- Avoid nested cards. Use cards only for snapshot cells, repeated map items, and collapsible proof blocks.
- Prefer full-width bands or unframed sections for major report regions.
- Tables should preserve readable column widths. This is a desktop report, so horizontal table overflow inside the report frame is acceptable when needed.
- Keep diagram frames stable with a visible border, subtle shadow, and no layout shift.

## Visual Language

Default style if no `references/design-style/` file is selected:

```css
:root {
  --paper: #fbfcfe;
  --panel: #ffffff;
  --ink: #172033;
  --muted: #5d6b82;
  --line: #d8e2ee;
  --quiet: #eef3f8;
  --accent: #1e40af;
  --accent-2: #3b82f6;
  --success: #047857;
  --warning: #b45309;
  --danger: #dc2626;
  --radius: 8px;
  --shadow: 0 16px 42px rgba(15, 23, 42, .06);
}
```

Rules:

- Use tokens for colors, radius, spacing, and shadows.
- Do not use gradient text, glassmorphism, decorative blobs, or generic SaaS hero patterns.
- Use color semantically: blue for structure/control, green for output/recovery success, amber for state/risk interpretation, red for failure/recovery warnings.
- Use one accent family plus semantic colors. Do not make the report a one-hue poster.
- Use subtle shadows instead of heavy borders for elevated blocks, but keep table borders crisp.
- Use `border-radius: 8px` by default unless a selected style file says otherwise.

## Interaction Rules

The report should feel alive without becoming an app.

Required:

- Sticky or easy-to-scan table of contents only when the report is long enough to need it.
- `<details>` disclosure for optional evidence.
- Hover/focus states on artifact links and controls.
- `:focus-visible` styles with clear contrast.
- `prefers-reduced-motion` support.

Allowed:

- Scroll progress indicator.
- Back-to-top button.
- Copy buttons for code blocks.
- Expand/collapse controls for evidence groups.
- Lightweight diagram zoom only if it is implemented locally and does not obscure reading.

Motion:

- Animate only `opacity`, `transform`, `filter`, or color.
- Use exact transition properties, never `transition: all`.
- Keep most transitions between `140ms` and `260ms`.
- Use `cubic-bezier(0.2, 0, 0, 1)` or `cubic-bezier(0.22, 1, 0.36, 1)`.
- Disable spatial motion under `prefers-reduced-motion: reduce`.

## Report-To-HTML Transformation

When converting an existing Markdown report:

- Remove the duplicate Markdown H1 if the HTML hero already has one.
- Preserve headings, tables, code blocks, and lists.
- Replace any stale diagram description so it matches the actual artifact (`local SVG`, `brief`, optional `Mermaid`).
- Do not rewrite technical claims unless the source report contradicts the generated artifacts.
- Keep Chinese text as Chinese unless the user asks otherwise.
- Keep the default report sections from `report-template.md` in their original order unless the user explicitly asks for a different reading order.
- If a first-screen snapshot is added, derive it from `30 秒总览`, `一句话主线`, and `项目特殊火花`; do not make new architecture claims in the HTML layer.
- If a section is duplicated in the hero/snapshot, keep the original body section unless removing it would make the evidence layer clearer and no information is lost.
- Use anchors on headings so table-of-contents links can jump to real report sections.

## Desktop And Print

This report is desktop-only by default. Do not spend time creating narrow-viewport rules unless the user explicitly asks for them.

Desktop:

- Target comfortable reading at common desktop widths, roughly `1280px` and wider.
- Keep the architecture diagram large enough to inspect without zooming the browser page.
- Keep artifact links, table of contents, and evidence controls pointer-friendly.
- Avoid horizontal page overflow at desktop widths. Table-local overflow is acceptable.

Print, when easy:

- Remove sticky controls and heavy shadows.
- Keep the diagram and major tables from breaking awkwardly when practical.
- Preserve code block contrast enough to read on paper.

## Quality Gate

Before final delivery:

- Validate the HTML in a real browser, preferably with Chrome DevTools or the Browser/Chrome DevTools MCP when available.
- Do not capture image artifacts during default validation. Use DOM, console, network, computed layout, and element inspection instead.
- Check the console for errors.
- Check failed network/file requests.
- Inspect the DOM to confirm the architecture SVG is present and loaded.
- Inspect layout at a desktop viewport and confirm no major text overlap or broken report frames.
- Confirm the architecture SVG loads.
- Confirm the first screen answers the anatomy snapshot questions.
- Confirm artifact links point to existing files.
- Confirm the report still includes module-to-file and folder/module analysis.
