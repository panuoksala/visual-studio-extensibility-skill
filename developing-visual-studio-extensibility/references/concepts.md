# Concepts

## Mental model

- **VisualStudio.Extensibility is primarily out-of-process.** Extensions run in a separate process
  from `devenv.exe`. This is the defining architectural difference from the classic VSSDK model.
  It enables install-without-restart and improves host stability, but it also means not all VS
  APIs are available at the same surface area as in-process extensions.

- **Contributions are how extensions light up functionality.** Every piece of extension behavior
  (commands, tool windows, editor margins, etc.) is declared as a *contribution*. Classes that
  provide contributions are decorated with `[VisualStudioContribution]` and discovered
  automatically by the framework. There is no `.vsct` file; configuration is code-based.

- **Activation constraints control when functionality appears.** Contributions declare when they
  are visible or enabled via `ActivationConstraint` expressions (`VisibleWhen`, `EnabledWhen`).
  This replaces context GUIDs and VSCT visibility rules from the classic model.

## Core concepts to mention when relevant

- **Extension anatomy** — every extension has a root class that extends `Extension`. Override
  `InitializeServices` to register dependencies. The framework discovers contributed types
  automatically; manual registration in a package initializer is not required.

- **Dependency injection** — the framework provides a built-in DI container. Services are
  injected into contributed classes through constructor parameters. Do not use `ServiceProvider`
  or `GetService` patterns from the classic VSSDK; use constructor injection instead.

- **`this.Extensibility` façade** — contributed classes access all VS services through the
  `VisualStudioExtensibility` object, typically available as `this.Extensibility`. It is the
  single entry point for views, editor access, output window, project query, and more. Steer
  users toward it instead of service-locator patterns.

- **`IClientContext`** — commands and other handlers receive an `IClientContext` parameter that
  captures the UI state at the moment the user triggered the action (active document, selection,
  etc.). Guide users to read state from `IClientContext` inside the handler rather than caching
  VS state in fields.

- **Async everywhere** — all VS service calls are cross-process and must be awaited. Blocking
  on async code (`.Result`, `.Wait()`) will deadlock. When users ask why their extension hangs,
  this is the first thing to check.

- **Remote UI** — tool windows and dialogs that run out-of-process use *Remote UI*, a
  WPF-compatible data-binding model where the view runs in the VS process but the view-model
  runs in the extension process. XAML resources and converters must be declared in a way that
  the Remote UI bridge can serialize them.

- **Stable versus experimental APIs** — some VisualStudio.Extensibility APIs are stable; others
  are annotated with `[Experimental]`. Experimental APIs can change or be removed in any
  preview release. When recommending an API that carries `[Experimental]`, say so explicitly
  and link to the breaking changes page:
  `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`

- **Extension manifest and metadata** — extension identity (name, publisher, version, minimum
  VS version) is declared in the project file and drives the generated manifest. If the extension
  fails to load silently after install, a malformed or missing manifest is a common cause. Point
  users to the project properties and the build output for manifest errors.

- **Source generators power contribution discovery** — the SDK uses Roslyn source generators to
  scan `[VisualStudioContribution]` types and emit the manifest at build time. If a contribution
  is not appearing, a missing rebuild or disabled source generators in the project are likely
  culprits — not a runtime registration issue.

## When hybrid matters

If the requested capability is not yet available through VisualStudio.Extensibility, say so
explicitly and route to `references/hybrid-and-migration.md` rather than silently switching
to classic SDK guidance.

Common triggers for the hybrid path:
- The user needs a VS API that has no VisualStudio.Extensibility equivalent yet.
- The user asks about language services or editor APIs that are not fully covered in the
  current preview.
- The user needs to call into `IVsPackage`-based services or use COM interop with VS internals.

In those cases: acknowledge the gap, and read `references/hybrid-and-migration.md` **before**
describing which hybrid model to use. There are two supported hybrid architectures — do not
conflate them or default to one without consulting that file:

1. **Model 1 — In-process hosting** (`RequiresInProcessHosting = true`): the entire extension
   runs inside `devenv.exe` using the `VisualStudio.Extensibility Extension with VSSDK
   Compatibility` project template.
2. **Model 2 — Composite package**: a single VSIX that combines an out-of-proc
   VisualStudio.Extensibility component with a separate classic in-proc VSSDK component.

`references/hybrid-and-migration.md` defines both models, their trade-offs, and when each
applies. Do not silently produce classic VSSDK guidance as if it were VisualStudio.Extensibility
guidance.

## Feature-area routing and sources of truth

When answering a capability request (commands, tool windows, editor features, debugger
visualizers, settings, etc.), route to `references/feature-areas.md` for deeper detail rather
than falling back to generic VSIX or Community Toolkit guidance. High-level routing is defined
in `SKILL.md`.

Do not conflate VisualStudio.Extensibility feature areas with their VSSDK or Community Toolkit
counterparts; each is a separate API surface.

When steering a user toward an implementation, prefer these sources in order:

1. **Official Microsoft Learn documentation** for VisualStudio.Extensibility:
   `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/`
2. **`microsoft/VSExtensibility` sample repository** on GitHub:
   `github.com/microsoft/VSExtensibility/tree/main/New_Extensibility_Model/Samples`
3. Breaking-changes and known-issues pages in the same repo before citing workarounds.

Do not cite unrelated VSIX templates, Community Toolkit samples, or third-party blog posts as
primary implementation guidance for VisualStudio.Extensibility.
