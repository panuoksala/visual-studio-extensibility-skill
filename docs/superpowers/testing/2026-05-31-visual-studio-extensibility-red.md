# Visual Studio Extensibility Skill RED Notes

**Date:** 2026-05-31
**Status:** RED — baseline without skill
**Tester:** Copilot CLI (automated baseline run)

These notes record what a fresh agent produces for the three baseline scenarios when the
`developing-visual-studio-extensibility` skill does **not** exist. They justify every gap
listed in the Failure patterns section at the bottom.

---

## Scenario 1: Create an extension

**Prompt:** How do I create a Visual Studio extension with the new VisualStudio.Extensibility framework?

**Observed behavior:**

The agent returns a mix of Community Toolkit VSIX guidance and VisualStudio.Extensibility
guidance without distinguishing between them. The first search hit it acts on is titled
"Your first Visual Studio extension" — that article walks through the Community Toolkit
approach (not the new out-of-proc framework). The agent consequently suggests:

- Use the **VSIX Project** template (incorrect; the correct template is
  **VisualStudio.Extensibility Project**).
- Add a `.vsct` file to define commands (incorrect; the new framework uses code-based
  `CommandConfiguration` only).
- Reference `IVsPackage` or `Package` base class boilerplate from VSSDK (incorrect
  entrypoint for the new model; the new model requires extending the `Extension` base class
  with `[VisualStudioContribution]`).

What the agent omits entirely:

- The out-of-proc hosting model (extensions run outside `devenv.exe`), which is the
  defining architectural property of VisualStudio.Extensibility.
- The `[VisualStudioContribution]` attribute and its role in registering contributions.
- The `Extension.InitializeServices` override for dependency injection.
- The minimum Visual Studio requirement: VS 2022 version **17.9 Preview 1 or higher** with
  the `Visual Studio extension development` workload.
- The Preview status of the framework itself and the existence of the `[Experimental]`
  attribute on certain APIs.
- The canonical sample entry point: **SimpleRemoteCommandSample**
  (`New_Extensibility_Model/Samples/SimpleRemoteCommandSample`) in the
  `microsoft/VSExtensibility` GitHub repo.
- The official learn path:
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/get-started/create-your-first-extension`.

**Gaps:**

1. Framework naming confusion — conflates "new VisualStudio.Extensibility" with the
   Community Toolkit VSIX approach.
2. Wrong project template name surfaced.
3. No mention of out-of-proc hosting or what it means for reliability and installability.
4. No mention of the `[VisualStudioContribution]` / `Extension` base-class pattern.
5. No version prerequisite stated.
6. No preview-status caveat or pointer to known issues / breaking changes pages.
7. No official sample mapped.

---

## Scenario 2: Create a command

**Prompt:** How can I create a command with VisualStudio.Extensibility?

**Observed behavior:**

The agent lands on the Community Toolkit article "Adding menus & commands to Visual Studio
extensions" — the top search result for command creation in VS extensions — and produces
guidance for the **old** VSCT-based model:

- "Define the command in a `.vsct` file" (the new model never uses `.vsct`).
- "Implement `ICommandHandler`" or use `OleMenuCommandService` patterns from VSSDK
  (neither exists in the VisualStudio.Extensibility surface).
- No mention of the base class to extend (`Command`).

Key implementation details not surfaced:

- The correct pattern: extend `Command`, decorate with `[VisualStudioContribution]`, override
  the `CommandConfiguration` property, implement `ExecuteCommandAsync`.
- `CommandConfiguration` handles placement, icon, tooltip, shortcuts, and visibility — all
  in C#, with no `.vsct` file required.
- `CommandPlacement.KnownPlacements` — the typed API for placing commands under
  `ExtensionsMenu`, `ToolsMenu`, `ViewOtherWindowsMenu`.
- `EnabledWhen` / `VisibleWhen` activation constraints via `ActivationConstraint`.
- The fully async execution model (`Task ExecuteCommandAsync`), which is a first-class
  design goal of the framework.
- The `CommandParentingSample` and `InsertGuid` samples as reference implementations.
- Localization via `%...%` string tokens referencing `.vsextension/string-resources.json`.

**Gaps:**

1. Feature-area routing failure — routes to Community Toolkit command article instead of
   VisualStudio.Extensibility commands doc.
2. Incorrect implementation pattern (`.vsct` + `ICommandHandler` vs `Command` base class +
   `CommandConfiguration`).
3. No typed placement API mentioned.
4. No activation constraints mentioned.
5. No async model explained.
6. No sample mapped (`CommandParentingSample`, `InsertGuid`, `SimpleRemoteCommandSample`).

---

## Scenario 3: Choose framework model

**Prompt:** Should I use VisualStudio.Extensibility or the classic VSIX/VSSDK APIs for this feature?

**Observed behavior:**

The comparison page (`extensibility-models?view=visualstudio`) does appear in search results,
so the agent provides a partial answer that is more accurate than scenarios 1 and 2. It
correctly notes:

- VSSDK runs in-proc; VisualStudio.Extensibility runs out-of-proc by default.
- VisualStudio.Extensibility requires VS 2022 and does not support VS 2019 or below.
- VisualStudio.Extensibility offers install-without-restart and .NET (not .NET Framework)
  targeting.

However the agent does not produce:

- **Hybrid/in-proc guidance** — the critical "if VisualStudio.Extensibility doesn't yet
  cover what you need, run in-proc" path. The agent does not explain
  `RequiresInProcessHosting = true`, the
  `VisualStudio.Extensibility Extension with VSSDK Compatibility` project template, or the
  fact that in-proc mode forces .NET Framework (same process as VS).
- **Three-model framing** — the agent presents a binary VSSDK vs. VisualStudio.Extensibility
  choice and omits the Community Toolkit as the third model, which matters for users with
  existing Community Toolkit extensions.
- **Feature-gap enumeration** — the agent does not name specific feature areas where
  VisualStudio.Extensibility coverage is still incomplete (e.g., some editor APIs,
  language services not fully covered yet) or point to the official feature parity tracking.
- **Preview caveat** — VisualStudio.Extensibility is still labeled Preview; the agent does
  not mention this, nor the `[Experimental]` attribute marking for unstable APIs, nor the
  breaking changes page at `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`.
- **Decision flow** — the comparison article contains a numbered decision tree (scenarios
  1–4 with specific recommendations). The agent summarizes loosely rather than routing to the
  structured decision tree.
- **CompositeExtension sample** — the canonical sample showing in-proc and out-of-proc
  components coexisting (`New_Extensibility_Model/Samples/CompositeExtension`) is not
  mentioned.

**Gaps:**

1. Missing hybrid guidance — `RequiresInProcessHosting`, in-proc project template, .NET
   Framework constraint for in-proc.
2. Binary framing misses Community Toolkit as a distinct third model.
3. Feature-gap enumeration absent — agent doesn't tell the user how to find out whether
   their required feature area is covered.
4. Preview caveat and `[Experimental]` attribute not mentioned.
5. Structured decision tree from the comparison article not reproduced or linked.
6. No hybrid sample mapped (`CompositeExtension`).

---

## Failure patterns

The three scenarios above reveal five recurring failure patterns that the
`developing-visual-studio-extensibility` skill must fix:

- **Framework naming confusion:** A fresh agent conflates VisualStudio.Extensibility with the
  Community Toolkit VSIX approach. The top Microsoft Learn search results for generic "create
  VS extension" and "VS extension command" queries surface the Community Toolkit articles
  first. Without explicit disambiguation, the agent routes users to the wrong programming
  model — wrong project template, wrong base classes, wrong configuration mechanism (`.vsct`
  vs. code-based `CommandConfiguration`).

- **Missing feature-area routing:** Even when a request names a specific capability (e.g.,
  "create a command"), the agent does not route to the corresponding VisualStudio.Extensibility
  feature-area doc. It lands on the first matching search result regardless of which framework
  model that article targets. The skill must map each feature area (commands, tool windows,
  dialogs, editor, output window, debugger visualizers, project query, language server,
  settings) to the correct VisualStudio.Extensibility documentation entry point.

- **Missing hybrid guidance:** The agent skips the hybrid/in-proc option entirely unless the
  user explicitly asks about it. Because VisualStudio.Extensibility is still in Preview and
  has feature gaps, the hybrid path (`RequiresInProcessHosting = true`, VSSDK compatibility
  template, .NET Framework constraint) is frequently the correct recommendation. The skill
  must teach the agent to check for feature-gap scenarios and proactively surface the
  hybrid option.

- **Missing sample mapping:** Official Microsoft samples in the `microsoft/VSExtensibility`
  GitHub repo (`SimpleRemoteCommandSample`, `InsertGuid`, `CommandParentingSample`,
  `ToolWindowSample`, `DialogSample`, `UserPromptSample`, `WordCountMargin`,
  `RegexMatchDebugVisualizer`, `VSProjectQueryAPISample`, `CompositeExtension`,
  `RustLanguageServerProvider`) are never referenced. These samples are the canonical
  starting points for each feature area, but the agent does not know to map a user goal
  to a specific sample. The skill must provide this goal→sample mapping.

- **Missing preview or known-issue caveats:** The agent never surfaces:
  - The "VisualStudio.Extensibility (Preview)" status of the framework.
  - The `[Experimental]` attribute marking for unstable APIs.
  - The minimum Visual Studio version requirement (VS 2022 17.9 Preview 1+).
  - The breaking changes page
    (`github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`).
  - The known issues tracker
    (`github.com/microsoft/VSExtensibility/blob/main/docs/known_issues.md`).
  Omitting these caveats causes developers to waste time on dead ends or unexplained build
  failures. The skill must inject these reminders whenever feature implementation or
  framework-choice guidance is given.
