# Source Structure

Use this reference when source files are available. The goal is to connect the repository's real folders to the architecture, so the user can find every concept in code.

## Folder-To-Module Analysis

Do not stop at a high-level architecture map. Explain the actual source tree:

| Folder/path | What lives here | Architecture module | Why it exists | Start reading here |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

Rules:

- Analyze meaningful top-level and important nested folders.
- Skip generated output, build artifacts, vendor folders, cache folders, and obvious assets unless they matter architecturally.
- Do not describe a folder as "contains X files" only. Explain what responsibility it owns.
- If a folder is a mixed bucket, say so and name the different responsibilities inside it.
- If an architecture module spans multiple folders, say which folder owns the center and which folders are adapters or extensions.

## Folder Deep-Dive Pattern

For each important folder, answer:

```text
Folder: [path]
Role: [what module or capability this represents]
Contains: [main file types/classes/subfolders]
Why it exists: [product/system need]
Connects to: [upstream/downstream modules]
Start reading: [first file/function/class]
Risks: [state/resource/coupling/confusion]
```

## Required Output Sections

When source files are available, include both:

1. **模块到文件夹/文件映射**
   Maps architecture nodes to concrete repository locations.

2. **关键文件夹剖析**
   Explains important folders in human language.

These are different:

- The module map answers: "Where is this architecture node in the repo?"
- The folder analysis answers: "When I open this folder, what am I looking at and why does it exist?"

## Sanity Check

The user should be able to open the repository after reading this section and know:

- which folder to open first
- which folders are core vs adapters vs extensions
- which folders are product-specific increments
- which folders contain risky shared resources or state
- which architecture node each folder belongs to

