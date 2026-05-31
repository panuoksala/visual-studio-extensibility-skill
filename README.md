# Visual Studio Extensibility Skill

A reusable agent skill for working with the **VisualStudio.Extensibility** framework, the newer model for building Visual Studio extensions.

It helps an agent:

- route questions to the right framework area
- distinguish **VisualStudio.Extensibility** from classic **VSSDK/VSIX** guidance
- point to the right official Microsoft Learn articles and sample projects
- explain when pure out-of-proc development is enough and when a hybrid or classic approach is required

## What is included

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

Supporting design and validation notes are under `docs\superpowers\`.

## Skill scope

The skill is designed for requests about:

- commands
- tool windows
- dialogs and prompts
- output window usage
- editor and document features
- debugger visualizers
- project query
- settings
- language server provider work
- hybrid or migration decisions between VisualStudio.Extensibility and classic VSIX/VSSDK

## Reference map

| File | Purpose |
| --- | --- |
| `SKILL.md` | Entry point, routing rules, and framework boundary checks |
| `references\install-and-prereqs.md` | Environment, preview, and setup assumptions |
| `references\concepts.md` | Architecture, contributions, activation, dependency injection, Remote UI |
| `references\feature-areas.md` | Capability-specific routing across the major framework areas |
| `references\samples-and-patterns.md` | Official sample mapping for common goals |
| `references\hybrid-and-migration.md` | When to stay pure out-of-proc and when hybrid/classic integration is needed |
| `references\debugging-packaging-and-gotchas.md` | Validation, debugging, packaging, and caveats |

## Repository contents

- `developing-visual-studio-extensibility\` - the actual skill package source
- `docs\superpowers\specs\` - approved design spec
- `docs\superpowers\plans\` - implementation plan
- `docs\superpowers\testing\` - RED/GREEN validation notes

## Packaging

The skill is intended to be packaged into a `.skill` artifact with the `package_skill.py` workflow from the skill authoring toolchain.

## License

MIT
