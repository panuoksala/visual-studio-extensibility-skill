# Debugging, packaging, and gotchas

## Debugging

- **Experimental instance** — VisualStudio.Extensibility extensions are debugged by launching
  VS 2022 in its Experimental Instance (`/RootSuffix Exp`). The default F5 launch profile from
  the VisualStudio.Extensibility project template configures this automatically. Do not debug
  against the main VS instance; the extension manifest may not be registered there.
- **Verify the extension loads** — after the Experimental Instance opens, check
  **Extensions → Manage Extensions** to confirm the extension appears and is enabled. If it
  does not appear, check the build output for manifest errors or missing `[VisualStudioContribution]`
  attributes. For out-of-proc extensions, confirm the extension host process
  (`Microsoft.VisualStudio.Extension.HostApp.exe` or similar) started in Task Manager.
- **Attaching the debugger** depends on the hosting model:
  - *Out-of-proc (default):* F5 from the project attaches automatically. If detached, use
    **Debug → Attach to Process** and select the extension host process
    (`Microsoft.VisualStudio.Extension.HostApp.exe` or similar).
  - *In-proc (`RequiresInProcessHosting = true`):* There is no separate extension host
    process. Attach to `devenv.exe` (the experimental instance) directly via
    **Debug → Attach to Process**.
- **Output window / diagnostics** — use `this.Extensibility.Views().Output()` to write
  diagnostic messages to the VS Output window from extension code. Avoid `Debug.WriteLine`;
  it goes to the extension process's own debug output, not to the VS Output window.

## Packaging

- **Package only after the extension validates cleanly.** Before creating a `.vsix`, confirm the
  extension builds without warnings on the `[VisualStudioContribution]` or manifest side.
  Malformed manifests cause silent load failures in the installed extension.
- **Keep the extension project free of unrelated files.** The `.vsix` includes everything the
  project outputs. Keep test projects, sample data, and development-only assets in a sibling project outside
  the VSIX project when possible. If you must reference such files inside the VSIX project,
  mark them with `<IncludeInVSIX>false</IncludeInVSIX>` to exclude them from the package.
- **Target framework constraint for hybrid.** If `RequiresInProcessHosting = true`, the
  packaged assembly must target .NET Framework. Attempting to package a `net8.0` or `net9.0`
  binary in an in-proc extension will fail at runtime.
- **VS Marketplace upload** — the `.vsix` is uploaded at `marketplace.visualstudio.com/manage`.
  Tag the listing with "VisualStudio.Extensibility" and "Preview" while the framework remains
  in Preview status so users know the extension requires VS 2022 17.9+ and accepts the Preview
  stability caveats.

## Gotchas

- **Preview status matters.** VisualStudio.Extensibility is still labeled Preview. The API
  surface can change between VS releases. Always declare the minimum VS version in the extension
  manifest and test against the specific VS version the user targets.
- **Experimental APIs can change.** Any API marked with the `[Experimental]` attribute is
  explicitly unstable and may be removed or replaced without a deprecation cycle. Mention this
  whenever you reference an `[Experimental]` API — the user should not build production features
  on it without accepting the risk.
- **Known issues and breaking changes must be surfaced when relevant.** Before recommending
  a pattern, check:
  - Breaking changes: `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`
  - Known issues: `github.com/microsoft/VSExtensibility/blob/main/docs/known_issues.md`
  If the user's VS version or SDK version intersects a known issue or breaking change, call it
  out explicitly before walking through implementation steps.
- **Async everywhere.** The VisualStudio.Extensibility API is fully async. Blocking on async
  calls (`.Result`, `.Wait()`) inside extension code will deadlock. All VS service calls must
  be awaited.
- **No direct `IServiceProvider` access in out-of-proc mode.** Out-of-proc extensions do not
  have access to the VS `IServiceProvider`. Use the typed VisualStudio.Extensibility API
  surface (`this.Extensibility`) instead. If the required service has no wrapper, this is a
  signal that hybrid mode may be needed.

## Verification checklist

- Correct framework identified (out-of-proc VisualStudio.Extensibility vs. hybrid vs. classic
  VSSDK vs. Community Toolkit)
- Correct reference file chosen (see routing guide in `SKILL.md`)
- Correct sample family mentioned (match the feature area to the sample in `samples-and-patterns.md`)
- Hybrid requirement called out when needed (capability gap confirmed against feature-areas
  docs and known-issues page)
- Preview or breaking-change caveat mentioned when applicable (Preview status, `[Experimental]`
  attribute, breaking changes page, minimum VS version)
