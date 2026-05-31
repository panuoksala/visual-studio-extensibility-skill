# Sample Coverage Extension Design

## Overview

Extend the Visual Studio Extensibility skill so its sample guidance reflects the broader official sample set surfaced from Microsoft Learn, not just the currently listed hand-picked examples. The update should make it easier for an agent to discover the right official sample from both the central sample catalog and the most relevant feature-area routes.

## Goal

Improve `samples-and-patterns.md` and selected parts of `feature-areas.md` so the skill points to a broader Learn-backed set of official `microsoft/VSExtensibility` samples, while keeping the references concise and capability-focused.

## Non-goals

- Do not turn `samples-and-patterns.md` into a full dump of every sample in the repository.
- Do not rewrite the overall skill structure.
- Do not add new top-level reference files.
- Do not broaden the README unless the repository’s top-level positioning materially changes.

## Files in Scope

- `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\samples-and-patterns.md`
- `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\feature-areas.md`
- `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-green.md` (only if validation conclusions materially change)
- `I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill` (rebuilt artifact)

## Source of Truth

Primary source of truth:

- Microsoft Learn: `About VisualStudio.Extensibility (Preview)` samples and tutorials table
- Microsoft Learn: `Create a simple extension`
- The linked `microsoft/VSExtensibility` sample paths under `New_Extensibility_Model/Samples/`

The implementation should prefer Learn-backed sample names and repository paths already exposed from official docs.

## Proposed Changes

### 1. Expand `samples-and-patterns.md`

Keep the existing goal-to-sample table, but extend it with additional official samples that are currently missing or underrepresented in the file.

Likely additions include:

- `OutputWindowSample`
- `MarkdownLinter`
- `CommentRemover`
- `DocumentSelectorSample`
- `CommandParentingSample`

Existing command-oriented entries such as `SimpleRemoteCommandSample` and `InsertGuid` should remain, but they should no longer carry the implied weight of being the main path through the samples file.

Add a compact subsection after the table, such as `## Official sample catalog from Learn`, that:

- points to the Learn samples table as the discovery hub
- mentions `Samples.sln` as the umbrella solution for the official samples
- explains that the table above is a curated routing layer, not an exhaustive mirror

### 2. Add selective cross-references in `feature-areas.md`

Update only the feature sections where a Learn-backed sample materially improves routing clarity.

Expected targets:

- `## Commands`
- `## Editor and documents`
- `## Output window`

Potentially add one or two more only if the Learn-backed sample clearly improves discoverability without bloating the file.

The cross-references should be short and capability-oriented, for example:

- route command placement / parenting questions toward `CommandParentingSample`
- route output pane questions toward `OutputWindowSample`
- route document-scope applicability questions toward `DocumentSelectorSample`

Avoid turning `feature-areas.md` into a second sample catalog.

### 3. Keep the tutorial as supporting context

The `Create a simple extension` tutorial should still inform command/editor onboarding, but it should not become the center of the sample strategy.

Use it as supporting context where relevant:

- helpful for end-to-end command + editor-edit onboarding
- secondary to the broader official sample catalog

## Validation Plan

1. Verify each newly added sample name against Microsoft Learn and the linked GitHub sample path.
2. Confirm that the new entries improve routing rather than duplicating existing rows.
3. Refresh the GREEN validation notes only if the skill now advertises meaningfully broader routing coverage or if routing conclusions change.
4. Rebuild the `.skill` artifact so the packaged output matches the updated reference files.

## Constraints

- Keep the references concise.
- Preserve the current capability-first structure.
- Do not duplicate the same sample guidance across too many sections.
- Prefer official Microsoft Learn naming over invented aliases.

## Expected Outcome

After the update:

- `samples-and-patterns.md` better reflects the official Learn-backed sample set
- `feature-areas.md` more naturally routes users to the right sample from relevant feature sections
- the packaged `.skill` artifact matches the updated documentation
