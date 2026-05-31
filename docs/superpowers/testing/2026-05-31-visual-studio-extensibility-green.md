# Visual Studio Extensibility Skill GREEN Notes

**Date:** 2026-05-31
**Status:** GREEN — with skill active
**Tester:** Copilot CLI (Task 7 rerun from current corrected file state)

These notes record the results of running the three baseline scenarios against the
current state of all six skill files. Each scenario is evaluated by tracing the
SKILL.md routing guide against the full content of every referenced file, then
checking each RED gap individually. No skill-file edits were required before recording
results — all gaps were already resolved in the current file state.

---

## Scenario results

- **Scenario 1:** PASS — "How do I create a Visual Studio extension with the new VisualStudio.Extensibility framework?"

  With the skill active the agent consults `SKILL.md`, which routes "extension anatomy,
  contributions, activation, dependency injection" to `concepts.md` and "environment or
  version prerequisites" to `install-and-prereqs.md`. The sample table in
  `samples-and-patterns.md` maps "add a command" to `SimpleRemoteCommandSample`.

  RED gaps verified as resolved:

  1. **Framework naming confusion** — `install-and-prereqs.md` step 1 instructs the agent
     to confirm the user means the new out-of-proc framework (project type
     *VisualStudio.Extensibility Project*), not the Community Toolkit VSIX approach or
     classic VSSDK. Three separate programming models are named explicitly.
  2. **Wrong project template** — *VisualStudio.Extensibility Project* template name stated
     in `install-and-prereqs.md`.
  3. **Out-of-proc hosting omitted** — `concepts.md` leads with "VisualStudio.Extensibility
     is primarily out-of-process" and explains install-without-restart and host stability
     implications.
  4. **`[VisualStudioContribution]` / `Extension` base class omitted** — `concepts.md`
     covers the `Extension` root class, `InitializeServices` override, the
     `[VisualStudioContribution]` attribute, and automatic contribution discovery.
  5. **Version prerequisite absent** — `install-and-prereqs.md`: VS 2022 version 17.9
     Preview 1 or higher, *Visual Studio extension development* workload, preview NuGet
     packages (`Microsoft.VisualStudio.Extensibility`, `Microsoft.VisualStudio.Extensibility.Sdk`).
  6. **Preview caveat absent** — `install-and-prereqs.md` step 4 mandates proactive surfacing
     of Preview status, `[Experimental]` API risk, breaking-changes URL, and known-issues URL
     whenever implementation guidance is given.
  7. **No sample mapped** — `samples-and-patterns.md`: `SimpleRemoteCommandSample` listed
     under "Add a command"; official Learn `create-your-first-extension` URL also in
     `install-and-prereqs.md`.

- **Scenario 2:** PASS — "How can I create a command with VisualStudio.Extensibility?"

  `SKILL.md` routing table maps "Commands" explicitly to `feature-areas.md`. The Commands
  section of that file and the `samples-and-patterns.md` table cover all RED gaps.

  RED gaps verified as resolved:

  1. **Feature-area routing failure** — `SKILL.md` routing row for "Commands, tool windows,
     dialogs …" points unambiguously to `feature-areas.md`, not to generic VSIX articles.
  2. **Wrong implementation pattern** — `feature-areas.md` Commands section: "The `Command`
     base class replaces `.vsct` files entirely." Pattern stated as: extend `Command`,
     decorate with `[VisualStudioContribution]`, override `CommandConfiguration`, implement
     `ExecuteCommandAsync`.
  3. **No typed placement API** — `feature-areas.md`: `CommandPlacement.KnownPlacements`
     locations (`ExtensionsMenu`, `ToolsMenu`, `ViewOtherWindowsMenu`) listed; arbitrary
     menu injection flagged as requiring in-proc VSSDK.
  4. **No activation constraints** — `feature-areas.md`: `EnabledWhen`/`VisibleWhen` via
     `ActivationConstraint` documented as the replacement for classic context GUIDs and VSCT
     visibility rules.
  5. **No async model** — `feature-areas.md` mentions `ExecuteCommandAsync`;
     `debugging-packaging-and-gotchas.md` states "The VisualStudio.Extensibility API is fully
     async. Blocking on async calls will deadlock."
  6. **No sample mapped** — `samples-and-patterns.md`: "Add a command" →
     `SimpleRemoteCommandSample`, `CommandParentingSample`; "Add a command that also writes
     to the editor" → `InsertGuid`.

- **Scenario 3:** PASS — "Should I use VisualStudio.Extensibility or the classic VSIX/VSSDK APIs for this feature?"

  `SKILL.md` routing: "Hybrid or migration decisions" → `hybrid-and-migration.md`. That
  file is structured as an explicit decision guide and covers all RED gaps.

  RED gaps verified as resolved:

  1. **Hybrid guidance absent** — `hybrid-and-migration.md` "Hybrid setup essentials":
     Model 1 (`RequiresInProcessHosting = true`, *VisualStudio.Extensibility Extension with
     VSSDK Compatibility* template, .NET Framework constraint, restart-on-install); Model 2
     (`CompositeExtension` style, out-of-proc + classic in-proc in one VSIX). Both models
     described and distinguished.
  2. **Binary framing** — `hybrid-and-migration.md` "Migration cues" names Community Toolkit
     VSIX as the distinct third model; `install-and-prereqs.md` step 1 lists all three models
     explicitly.
  3. **Feature-gap enumeration absent** — `hybrid-and-migration.md` "When hybrid or classic
     VSSDK is required" points to `learn.microsoft.com/…/get-started/supported-capabilities`
     and the known-issues page to verify feature coverage before recommending hybrid.
  4. **Preview caveat absent** — `concepts.md` "Stable versus experimental APIs" and
     `install-and-prereqs.md` step 4 both surface `[Experimental]` annotation risk,
     breaking-changes URL, and known-issues URL.
  5. **No decision flow** — `hybrid-and-migration.md` "Model comparison reference" section
     points to the canonical comparison page
     `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/get-started/extensibility-models`
     with its numbered Scenarios 1–4 decision tree and three-model table.
  6. **No hybrid sample** — `hybrid-and-migration.md` Model 2 section names `CompositeExtension`
     (`New_Extensibility_Model/Samples/CompositeExtension`); `samples-and-patterns.md` also
     lists it under "Combine new and classic models."

---

## Improvements over RED

- **Routing improved:** All three scenarios route to the correct reference file on the first
  step via the `SKILL.md` routing table. Community Toolkit VSIX articles that topped generic
  search results in RED are preempted by explicit framework disambiguation in
  `install-and-prereqs.md` and `concepts.md`. The routing table includes "output window"
  as a keyword trigger, closing the gap where generic output-window queries might otherwise
  miss the feature-areas file.
- **Sample mapping improved:** Every major command scenario is covered — `SimpleRemoteCommandSample`
  and `CommandParentingSample` for standalone commands; `InsertGuid` for command + editor
  integration; `CompositeExtension` for the hybrid scenario. All samples attributed to
  `microsoft/VSExtensibility` under `New_Extensibility_Model/Samples/`.
- **Hybrid guidance improved:** `hybrid-and-migration.md` opens with a "Model comparison
  reference" section pointing to the official three-model comparison page and its numbered
  Scenarios 1–4 decision tree. Both hybrid architectural models are clearly distinguished
  with their separate constraints. Community Toolkit is named as the third model throughout.
- **Preview caveats improved:** `install-and-prereqs.md` step 4 mandates proactive surfacing
  of Preview status, `[Experimental]` annotation risk, minimum VS version (17.9 Preview 1),
  and both the breaking-changes page and known-issues tracker URLs whenever implementation
  guidance is given.

---

## Revisions still needed

- None.
