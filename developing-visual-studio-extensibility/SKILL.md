---
name: developing-visual-studio-extensibility
description: Use when working with VisualStudio.Extensibility, the new framework for developing Visual Studio extensions, including commands, tool windows, dialogs, settings, output window, editor or document features, debugger visualizers, project query, language server provider work, or when deciding between out-of-proc, hybrid, and classic VSIX approaches.
---

# Developing Visual Studio Extensibility

Use this skill to route requests about the new VisualStudio.Extensibility framework to the right reference material and official samples.

## Workflow

1. Confirm the request is about VisualStudio.Extensibility rather than the classic VSSDK alone.
2. Identify the user's immediate goal.
3. Read only the matching reference file.
4. Prefer Microsoft Learn and official samples as the primary source of truth.
5. Call out preview limitations, experimental APIs, known issues, and hybrid requirements when they affect the request.

## Routing guide

| Need | Read |
| --- | --- |
| Environment or version prerequisites | `references/install-and-prereqs.md` |
| Extension anatomy, contributions, activation, dependency injection, Remote UI | `references/concepts.md` |
| Commands, tool windows, dialogs, settings, output window, editor/document features, project query, debugger visualizers, language server provider | `references/feature-areas.md` |
| Best official sample or starting point | `references/samples-and-patterns.md` |
| Hybrid or migration decisions | `references/hybrid-and-migration.md` |
| Debugging, packaging, breaking changes, known issues | `references/debugging-packaging-and-gotchas.md` |

## Boundary checks

- If the user is asking about the new out-of-proc framework, stay in VisualStudio.Extensibility guidance.
- If the feature is not yet covered by the new framework, explain that hybrid or classic in-proc VSSDK may be required and then read `references/hybrid-and-migration.md`.
- Do not default to classic VSIX guidance without stating why the new framework is insufficient.
