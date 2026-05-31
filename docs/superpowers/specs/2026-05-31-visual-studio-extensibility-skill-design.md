# Visual Studio Extensibility Skill Design

## Overview

Create a standalone skill for developing Visual Studio extensions with the new **VisualStudio.Extensibility** framework. The skill should act as a concise router plus focused reference material, not as a long tutorial. It should help both developers who are new to the framework and experienced VSIX developers who need to map existing concepts to the newer model.

## Goals

- Help an agent recognize when a request is about **VisualStudio.Extensibility** rather than the classic VSSDK alone.
- Route the agent to the right conceptual or feature-area guidance with minimal context overhead.
- Align the skill structure with Microsoft's documentation model so the skill is easy to navigate and maintain.
- Cover common extension-development tasks such as commands, tool windows, dialogs, settings, editor/document features, project query, debugger visualizers, and language server provider work.
- Surface preview status, experimental APIs, known issues, breaking changes, and hybrid/in-proc guidance when they matter.

## Non-goals

- Do not act as a generic "getting started with extensions" tutorial.
- Do not assume the extension project is always created by this skill; users may scaffold or set up projects through other tools.
- Do not duplicate large amounts of Microsoft Learn content inline in `SKILL.md`.

## Proposed Skill Shape

### Skill name

`developing-visual-studio-extensibility`

### Top-level layout

```text
developing-visual-studio-extensibility/
├── SKILL.md
└── references/
    ├── install-and-prereqs.md
    ├── concepts.md
    ├── feature-areas.md
    ├── samples-and-patterns.md
    ├── hybrid-and-migration.md
    └── debugging-packaging-and-gotchas.md
```

## SKILL.md Role

`SKILL.md` should stay short and work primarily as a router.

It should:

1. Define clear trigger language for requests about the new VisualStudio.Extensibility framework.
2. Distinguish framework questions from classic VSIX/VSSDK-only requests.
3. Route the agent based on the user's immediate need:
   - understand the framework
   - add a capability such as a command or tool window
   - choose between out-of-proc, in-proc, or hybrid
   - find the right official sample, API surface, or limitation
4. Instruct the agent to prefer official Microsoft Learn structure and Microsoft samples as the primary source of truth.
5. Remind the agent to mention preview limitations, experimental APIs, known issues, and breaking changes when relevant.

## Reference File Responsibilities

### `references\install-and-prereqs.md`

- Supported Visual Studio versions
- Required workloads or environment prerequisites
- Preview assumptions
- Basic environment checks before attempting framework-specific work

### `references\concepts.md`

- Extension anatomy
- Contributions and configurations
- Activation constraints
- Dependency injection
- Remote UI
- Stable vs experimental API concepts

### `references\feature-areas.md`

- Commands
- Editor and document features
- Output window
- Tool windows
- User prompts and dialogs
- Debugger visualizers
- Project Query
- Settings
- Language Server Provider

Each section should focus on recognition, key implementation patterns, and common limitations rather than copying full tutorials.

### `references\samples-and-patterns.md`

- Map common user goals to the best official sample or documentation entry point
- Show which sample is the best starting point for commands, tool windows, dialogs, visualizers, project queries, and hybrid scenarios
- Highlight reusable implementation patterns inferred from the official samples

### `references\hybrid-and-migration.md`

- When the new framework is sufficient
- When hybrid/in-proc support is required
- How classic VSIX/VSSDK concepts map into the newer model
- How to reason about gaps in preview coverage

### `references\debugging-packaging-and-gotchas.md`

- Experimental instance workflow
- Debugging expectations
- Packaging and publishing notes
- Breaking changes and known issues
- Practical verification checklist for extension work

## Workflow the Skill Should Teach

When the skill triggers, the agent should:

1. Confirm the request targets the **new** VisualStudio.Extensibility framework.
2. Identify the immediate task or decision.
3. Load only the most relevant reference file.
4. Use official terminology and samples.
5. Call out framework limitations or hybrid requirements before suggesting an implementation path.

## Authoring Style

- Keep `SKILL.md` concise and heavily optimized for discovery.
- Put detailed framework knowledge in reference files.
- Use imperative guidance in the skill body.
- Prefer small decision flows and task routing over long narrative explanations.
- Avoid duplicating the same information across `SKILL.md` and references.

## Validation Plan

Follow a TDD-style skill authoring loop.

### RED

Before writing the skill, run baseline scenarios without it. Example scenarios:

- "How do I create a Visual Studio extension with the new framework?"
- "How can I create a command?"
- "Should I use VisualStudio.Extensibility or classic VSIX APIs for this feature?"

Capture where an agent fails to:

- recognize the framework correctly
- map requests to the right feature area
- identify hybrid/in-proc requirements
- use official samples and docs structure
- mention preview limitations, breaking changes, or known issues

### GREEN

Write the minimal skill and references needed to correct the baseline failures, then rerun the same scenarios with the skill present.

Success criteria:

- correct routing to the right reference file
- correct framework terminology
- relevant sample and documentation references
- clear handling of hybrid/preview limitations when applicable

### REFACTOR

Tighten wording and routing if later tests show ambiguity, over-loading of context, or weak distinction between VisualStudio.Extensibility and older VSSDK guidance.

## Packaging Plan

- Use `init_skill.py` only if scaffolding from the template is still helpful.
- Package with `package_skill.py`.
- Let packaging validation catch frontmatter, description, structure, and reference issues before final delivery.

## Notes

- The structure is intentionally aligned to the Microsoft Learn navigation model for VisualStudio.Extensibility.
- The design deliberately omits a dedicated getting-started tutorial file because users may begin from other tools or existing solutions.
