# Hybrid and migration

## Model comparison reference

When helping a user choose between models, consult the official comparison page first:
`learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/get-started/extensibility-models`

That page provides a numbered decision tree (Scenarios 1–4) and a side-by-side table of the three extensibility models (VisualStudio.Extensibility out-of-proc, VSSDK in-proc, Community Toolkit). Use it as the authoritative framing before explaining hybrid details.

## When VisualStudio.Extensibility is enough

- The requested feature exists in the new framework's documented feature areas (commands, tool
  windows, dialogs, editor/document access, output window, settings, debugger visualizers,
  project query, language server provider). Check `feature-areas.md` for the full list.
- The user targets VS 2022 17.9 Preview 1 or later exclusively and does not need to support
  any Visual Studio version earlier than VS 2022 17.9 Preview 1.
- The feature is listed in official Learn docs or is demonstrated in one of the canonical samples
  at `github.com/microsoft/VSExtensibility/tree/main/New_Extensibility_Model/Samples`.

## When hybrid or classic VSSDK is required

- **Capability gap** — the required VS API or feature area is not yet available in
  VisualStudio.Extensibility. Examples: arbitrary menu injection, some language service APIs,
  certain editor extension points. If unsure, check the feature-parity table at
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/get-started/supported-capabilities`
  and the known issues page at
  `github.com/microsoft/VSExtensibility/blob/main/docs/known_issues.md`.
- **In-process requirement** — the extension must access VS internals that are only available
  in-process (e.g., direct `IVsHierarchy` manipulation, certain shell services with no
  VisualStudio.Extensibility wrapper).
- **Explicit in-proc integration in docs or samples** — when the official sample or Learn article
  for the feature area explicitly uses the `VisualStudio.Extensibility Extension with VSSDK
  Compatibility` project template or sets `RequiresInProcessHosting = true`.

### Hybrid setup essentials

There are two distinct hybrid architectures; do not conflate them.

#### Model 1 — In-process hosting (`RequiresInProcessHosting = true`)

- Use the **VisualStudio.Extensibility Extension with VSSDK Compatibility** project template
  (not the standard VisualStudio.Extensibility Project template).
- Set `RequiresInProcessHosting = true` in the extension class. This runs the **entire**
  extension inside `devenv.exe`, giving direct access to the full VSSDK surface at the cost of
  requiring a restart on install/uninstall.
- In-proc mode forces **.NET Framework** targeting (same process as VS). Out-of-proc extensions
  target .NET (modern) instead.
- No canonical sample is cited here; if an official sample or Learn article for the target
  feature area explicitly uses this model, follow that guidance directly.

#### Model 2 — Combined out-of-proc + classic component in one VSIX

- A single VSIX packages both a VisualStudio.Extensibility (out-of-proc) component and a
  separate classic in-proc VSSDK component. The out-of-proc part runs in its own process; the
  classic part runs inside `devenv.exe`.
- The canonical sample is **CompositeExtension**
  (`New_Extensibility_Model/Samples/CompositeExtension`), which demonstrates this pattern.

## Migration cues for existing VSIX developers

- **Contributions replace several classic registration concepts.** `.vsct` files, `ProvideMenuResource`,
  context GUIDs, and `ProvideAutoLoad` attributes have no direct equivalents. Everything is
  expressed in C# via `[VisualStudioContribution]` and `CommandConfiguration` / `ActivationConstraint`.
- **Out-of-proc architecture changes assumptions about extension boundaries.** The extension
  process is separate from `devenv.exe`. Calls to VS services are async and cross-process by
  default. Code that relies on synchronous, in-proc VS service access must be moved to a hybrid
  component or replaced with VisualStudio.Extensibility async APIs.
- **Hybrid is a deliberate compatibility choice, not the default answer.** Start with pure
  out-of-proc VisualStudio.Extensibility. Introduce VSSDK compatibility only when you have
  confirmed that a specific required feature is not yet available in the new framework. If
  the chosen hybrid model is **Model 1** (`RequiresInProcessHosting = true`), the entire
  extension runs inside `devenv.exe`: it inherits .NET Framework targeting and loses
  install-without-restart. **Model 2** (`CompositeExtension` style) does not impose those
  constraints on the VisualStudio.Extensibility component itself; only the separate classic
  in-proc VSSDK component carries the .NET Framework requirement.
- **Community Toolkit VSIX** — The Community Toolkit for Visual Studio is a helper
  layer built on top of the classic VSSDK and follows the same in-proc architecture. If the
  user is migrating from a Community Toolkit extension, they are migrating from the classic
  in-proc model. Guide them to start with a new VisualStudio.Extensibility project rather
  than porting class by class.
- **Breaking changes page** — always check
  `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md` when migrating an
  existing extension to a newer SDK version; the Preview framework has made breaking changes
  between releases.
