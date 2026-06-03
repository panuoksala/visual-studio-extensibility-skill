# Visual Studio Extensibility Skill

A reusable agent skill for working with the **VisualStudio.Extensibility** framework, the newer model for building Visual Studio extensions.

It helps an agent:

- route questions to the right framework area
- distinguish **VisualStudio.Extensibility** from classic **VSSDK/VSIX** guidance
- point to the right official Microsoft Learn articles and sample projects
- explain when pure out-of-proc development is enough and when a hybrid or classic approach is required

## Installation

### Quick install (recommended)

Install globally for all your projects:

```bash
npx skills add panuoksala/visual-studio-extensibility-skill -g
```

Or install locally in a specific project:

```bash
npx skills add panuoksala/visual-studio-extensibility-skill
```

This works with any agent that supports the open skills ecosystem (Claude Code, Cursor, Windsurf, GitHub Copilot, and others). After installing, the skill is available automatically when you work on Visual Studio extension projects.

### Manual installation

If you prefer to install without the CLI, copy the `developing-visual-studio-extensibility\` folder into your agent's skills directory.

#### Claude Code

1. Copy `developing-visual-studio-extensibility\` to either:
   - `~/.claude/skills\developing-visual-studio-extensibility\` for a user-wide install, or
   - `<your-repo>\.claude\skills\developing-visual-studio-extensibility\` for a project-local install
2. Restart Claude Code or reload the project.
3. Run `/skills` and confirm `developing-visual-studio-extensibility` is available.

#### GitHub Copilot CLI

1. Copy `developing-visual-studio-extensibility\` to either:
   - `~/.copilot/skills\developing-visual-studio-extensibility\` for a user-wide install, or
   - `<your-repo>\.github\skills\developing-visual-studio-extensibility\` for a repo-local install
2. Run `/skills` to confirm the skill is available and enabled.

#### Visual Studio Code

VS Code can use the same guidance through GitHub Copilot custom instructions or prompt files.

1. Install the **GitHub Copilot** extension in Visual Studio Code.
2. Clone this repository and open it in VS Code, or copy the relevant guidance into your target repo.
3. Choose one of these integration approaches:
   - add the core guidance to `.github\copilot-instructions.md`, or
   - convert the routing guidance into prompt files under `.copilot\`
4. Use `developing-visual-studio-extensibility\SKILL.md` as the router and the files in `developing-visual-studio-extensibility\references\` as the source material for those instructions or prompts.

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
