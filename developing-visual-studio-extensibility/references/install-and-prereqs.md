# Install and prerequisites

## Minimum environment

- **Visual Studio 2022 version 17.9 Preview 1 or higher** — VisualStudio.Extensibility is a
  Preview framework and requires a recent VS 2022 preview or release that ships the preview SDK.
  Check the official prerequisites page:
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/get-started/create-your-first-extension`
- **`Visual Studio extension development` workload** — install via the Visual Studio Installer.
  This workload provides the project templates and VSIX tooling needed to build, deploy, and
  debug extensions.
- **VisualStudio.Extensibility SDK (preview NuGet packages)** — the framework ships as preview
  NuGet packages (`Microsoft.VisualStudio.Extensibility`, `Microsoft.VisualStudio.Extensibility.Sdk`).
  These are distinct from the classic VSSDK packages. Confirm the project references the correct
  packages before giving implementation guidance.

## Before giving framework-specific guidance

1. **Confirm the user targets VisualStudio.Extensibility** — verify they mean the new out-of-process
   framework (project type: *VisualStudio.Extensibility Project*), not the classic VSIX/VSSDK model
   or the Community Toolkit VSIX approach. These are three separate programming models; mixing
   guidance across them causes incorrect output.

2. **Confirm the environment can load preview SDK bits** — the user must be on a VS 2022 build
   that ships the required preview SDK. If they are on VS 2022 stable and have not updated
   recently, some APIs marked `[Experimental]` may not be available or may have breaking changes.

3. **Note that older or stable VS installs may require different guidance** — VS 2019 and below do
   not support VisualStudio.Extensibility at all. VS 2022 stable releases may lag behind the
   preview SDK; if the user reports missing types or compilation errors, ask them to verify their
   VS version and NuGet package version before diagnosing further.

4. **Surface preview caveats proactively** — whenever implementation guidance is given, remind the
   user that:
   - VisualStudio.Extensibility is still labeled **Preview**.
   - APIs annotated with `[Experimental]` are subject to breaking changes without notice.
   - The breaking changes page is at:
     `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`
   - The known issues tracker is at:
     `github.com/microsoft/VSExtensibility/blob/main/docs/known_issues.md`
