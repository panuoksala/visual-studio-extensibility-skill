# Visual Studio Extensibility Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build, test, and package a reusable skill named `developing-visual-studio-extensibility` for the new VisualStudio.Extensibility framework.

**Architecture:** Create one lean `SKILL.md` that routes requests into six focused reference files aligned to the Microsoft Learn documentation structure. Validate the skill with RED/GREEN scenario testing before packaging it into a distributable `.skill` file.

**Tech Stack:** Markdown, PowerShell, Python helper scripts, Microsoft Learn documentation, agent-based scenario testing

---

## File Structure

### Production files

- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\install-and-prereqs.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\concepts.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\feature-areas.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\samples-and-patterns.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\hybrid-and-migration.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\debugging-packaging-and-gotchas.md`

### Development artifacts

- Create: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-red.md`
- Create: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-green.md`
- Create: `I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill`

### Existing inputs

- Read: `I:\own\skills-visualstudio-extensibility\docs\superpowers\specs\2026-05-31-visual-studio-extensibility-skill-design.md`

### Notes

- This workspace is not currently a git repository, so commit steps are intentionally omitted until version control exists.
- Keep the skill folder free of testing notes or extra markdown files that are not part of the final package.

### Task 1: Capture RED baseline scenarios

**Files:**
- Create: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-red.md`
- Read: `I:\own\skills-visualstudio-extensibility\docs\superpowers\specs\2026-05-31-visual-studio-extensibility-skill-design.md`
- Test: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-red.md`

- [ ] **Step 1: Create the RED notes file**

```markdown
# Visual Studio Extensibility Skill RED Notes

## Scenario 1: Create an extension
Prompt: How do I create a Visual Studio extension with the new VisualStudio.Extensibility framework?
Observed behavior:
Gaps:

## Scenario 2: Create a command
Prompt: How can I create a command with VisualStudio.Extensibility?
Observed behavior:
Gaps:

## Scenario 3: Choose framework model
Prompt: Should I use VisualStudio.Extensibility or the classic VSIX/VSSDK APIs for this feature?
Observed behavior:
Gaps:

## Failure patterns
- Framework naming confusion:
- Missing feature-area routing:
- Missing hybrid guidance:
- Missing sample mapping:
- Missing preview or known-issue caveats:
```

- [ ] **Step 2: Run the baseline scenarios without the new skill**

Use three fresh agent runs that do **not** have this new skill available yet. Use these exact prompts:

```text
How do I create a Visual Studio extension with the new VisualStudio.Extensibility framework?
```

```text
How can I create a command with VisualStudio.Extensibility?
```

```text
Should I use VisualStudio.Extensibility or the classic VSIX/VSSDK APIs for this feature?
```

Expected: at least one run misses official doc structure, sample mapping, hybrid guidance, or preview limitations, which proves the skill has something concrete to improve.

- [ ] **Step 3: Fill the RED notes file with verbatim observations**

Capture the actual misses. Use wording like:

```markdown
Observed behavior: Answer described VSIX patterns generically and did not distinguish out-of-proc VisualStudio.Extensibility from classic in-proc SDK guidance.
Gaps:
- Did not point to official sample families
- Did not mention preview status
- Did not explain when hybrid is required
```

- [ ] **Step 4: Review the RED notes for repeated failures**

Check that the `Failure patterns` section names the exact gaps the skill must fix.

Expected: the file contains at least three concrete failure patterns that can be mapped to skill content.

### Task 2: Scaffold the skill folder and remove template noise

**Files:**
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\install-and-prereqs.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\concepts.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\feature-areas.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\samples-and-patterns.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\hybrid-and-migration.md`
- Create: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\debugging-packaging-and-gotchas.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`

- [ ] **Step 1: Initialize the skill from the generator**

Run:

```powershell
python "C:\Users\mino\.copilot\installed-plugins\anthropic-agent-skills\document-skills\skills\skill-creator\scripts\init_skill.py" developing-visual-studio-extensibility --path "I:\own\skills-visualstudio-extensibility"
```

Expected: a new folder named `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility` exists with `SKILL.md` plus example resource directories.

- [ ] **Step 2: Remove generated example files that are not part of the approved design**

Run:

```powershell
Get-ChildItem "I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility" -Recurse
```

Delete any placeholder examples that are not one of the approved files from the File Structure section above.

Expected: only `SKILL.md` and the six reference files remain in the skill package.

- [ ] **Step 3: Create the exact reference file set**

If any approved reference file is missing, create it with these headings:

```markdown
# Install and prerequisites
# Concepts
# Feature areas
# Samples and patterns
# Hybrid and migration
# Debugging, packaging, and gotchas
```

- [ ] **Step 4: Verify the folder shape matches the spec**

Run:

```powershell
Get-ChildItem "I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility" -Recurse | Select-Object FullName
```

Expected: the output shows `SKILL.md` and exactly six files under `references\`.

### Task 3: Write the routing `SKILL.md`

**Files:**
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`
- Read: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-red.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`

- [ ] **Step 1: Replace the template frontmatter with the final metadata**

Use this frontmatter exactly:

```markdown
---
name: developing-visual-studio-extensibility
description: Use when working with VisualStudio.Extensibility, the new framework for developing Visual Studio extensions, including commands, tool windows, dialogs, settings, editor or document features, debugger visualizers, project query, language server provider work, or when deciding between out-of-proc, hybrid, and classic VSIX approaches.
---
```

- [ ] **Step 2: Add a concise overview and routing workflow**

Start the body with:

```markdown
# Developing Visual Studio Extensibility

Use this skill to route requests about the new VisualStudio.Extensibility framework to the right reference material and official samples.

## Workflow

1. Confirm the request is about VisualStudio.Extensibility rather than the classic VSSDK alone.
2. Identify the user's immediate goal.
3. Read only the matching reference file.
4. Prefer Microsoft Learn and official samples as the primary source of truth.
5. Call out preview limitations, experimental APIs, known issues, and hybrid requirements when they affect the request.
```

- [ ] **Step 3: Add the routing table**

Include this table in `SKILL.md`:

```markdown
## Routing guide

| Need | Read |
| --- | --- |
| Environment or version prerequisites | `references/install-and-prereqs.md` |
| Extension anatomy, contributions, activation, dependency injection, Remote UI | `references/concepts.md` |
| Commands, tool windows, dialogs, settings, editor/document features, project query, debugger visualizers, language server provider | `references/feature-areas.md` |
| Best official sample or starting point | `references/samples-and-patterns.md` |
| Hybrid or migration decisions | `references/hybrid-and-migration.md` |
| Debugging, packaging, breaking changes, known issues | `references/debugging-packaging-and-gotchas.md` |
```

- [ ] **Step 4: Add the framework boundary rules**

Append:

```markdown
## Boundary checks

- If the user is asking about the new out-of-proc framework, stay in VisualStudio.Extensibility guidance.
- If the feature is not yet covered by the new framework, explain that hybrid or classic in-proc VSSDK may be required and then read `references/hybrid-and-migration.md`.
- Do not default to classic VSIX guidance without stating why the new framework is insufficient.
```

- [ ] **Step 5: Review for token efficiency**

Run:

```powershell
(Get-Content "I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md" | Measure-Object -Word).Words
```

Expected: `SKILL.md` stays concise enough to act as a router rather than a tutorial.

### Task 4: Write prerequisite and concepts references

**Files:**
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\install-and-prereqs.md`
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\concepts.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\install-and-prereqs.md`

- [ ] **Step 1: Write `install-and-prereqs.md`**

Use this structure:

```markdown
# Install and prerequisites

## Minimum environment
- Visual Studio 2022 with the current VisualStudio.Extensibility preview requirements from Microsoft Learn
- `Visual Studio extension development` workload
- VisualStudio.Extensibility SDK preview assumptions

## Before giving framework-specific guidance
- Confirm the user is targeting the new VisualStudio.Extensibility framework
- Confirm the environment can load preview SDK bits
- Point out that older stable Visual Studio installs may require different guidance
```

- [ ] **Step 2: Write `concepts.md`**

Use this structure:

```markdown
# Concepts

## Mental model
- VisualStudio.Extensibility is primarily out-of-process
- Contributions are how extensions light up functionality
- Activation constraints control when functionality appears

## Core concepts to mention when relevant
- Extension anatomy
- Dependency injection
- Remote UI
- Stable versus experimental APIs
```

- [ ] **Step 3: Add a "when hybrid matters" subsection to `concepts.md`**

Append:

```markdown
## When hybrid matters

If the requested capability is not yet available through VisualStudio.Extensibility, say so explicitly and route to `references/hybrid-and-migration.md` rather than silently switching to classic SDK guidance.
```

- [ ] **Step 4: Review both files against the RED notes**

Expected: each repeated failure from Task 1 is now addressed by either `install-and-prereqs.md` or `concepts.md`.

### Task 5: Write feature routing and sample mapping references

**Files:**
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\feature-areas.md`
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\samples-and-patterns.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\feature-areas.md`

- [ ] **Step 1: Write `feature-areas.md` with one section per capability**

Use these exact section headings:

```markdown
# Feature areas

## Commands
## Editor and documents
## Output window
## Tool windows
## User prompts and dialogs
## Debugger visualizers
## Project query
## Settings
## Language Server Provider
```

- [ ] **Step 2: In each feature section, add the same four bullets**

Use this pattern:

```markdown
- What this area is for
- When to route here
- Official Learn article to consult first
- Common limitation or caveat to mention
```

- [ ] **Step 3: Write `samples-and-patterns.md` as a goal-to-sample map**

Start with this table:

```markdown
# Samples and patterns

| Goal | Start here |
| --- | --- |
| Add a command | SimpleRemoteCommandSample |
| Add editor text manipulation | InsertGuid |
| Add a tool window | ToolWindowSample |
| Show a prompt | UserPromptSample |
| Show a dialog | DialogSample |
| Build a debugger visualizer | RegexMatchDebugVisualizer or MemoryStreamDebugVisualizer |
| Query project data | VSProjectQueryAPISample |
| Combine new and classic models | CompositeExtension |
```

- [ ] **Step 4: Add a short pattern note under the table**

Append:

```markdown
## Pattern notes

- Prefer the official sample that matches the user's immediate goal instead of combining multiple samples too early.
- Mention contribution placement and activation constraints when they affect commands or UI.
- Use migration or hybrid guidance when the sample requires classic SDK integration.
```

- [ ] **Step 5: Review both files for coverage**

Expected: a request about commands, tool windows, dialogs, project queries, or hybrid scenarios can be routed without consulting unrelated sections.

### Task 6: Write hybrid, migration, debugging, and packaging references

**Files:**
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\hybrid-and-migration.md`
- Modify: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\debugging-packaging-and-gotchas.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\hybrid-and-migration.md`

- [ ] **Step 1: Write `hybrid-and-migration.md`**

Use this structure:

```markdown
# Hybrid and migration

## When VisualStudio.Extensibility is enough
- The requested feature exists in the new framework's documented feature areas

## When hybrid or classic VSSDK is required
- The capability is not yet available in the new framework
- The docs or samples explicitly rely on in-proc integration

## Migration cues for existing VSIX developers
- Contributions replace several classic registration concepts
- Out-of-proc architecture changes assumptions about extension boundaries
- Hybrid is a deliberate compatibility choice, not the default answer
```

- [ ] **Step 2: Write `debugging-packaging-and-gotchas.md`**

Use this structure:

```markdown
# Debugging, packaging, and gotchas

## Debugging
- Experimental instance expectations
- Verify the extension loads in the intended environment

## Packaging
- Package only after the skill validates cleanly
- Keep the skill folder free of unrelated files

## Gotchas
- Preview status matters
- Experimental APIs can change
- Known issues and breaking changes must be surfaced when relevant
```

- [ ] **Step 3: Add a verification checklist to `debugging-packaging-and-gotchas.md`**

Append:

```markdown
## Verification checklist

- Correct framework identified
- Correct reference file chosen
- Correct sample family mentioned
- Hybrid requirement called out when needed
- Preview or breaking-change caveat mentioned when applicable
```

- [ ] **Step 4: Cross-check the routing links**

Expected: every file referenced by `SKILL.md` exists and every "go read X" instruction points to a real file path.

### Task 7: Run GREEN scenario tests and tighten wording

**Files:**
- Create: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-green.md`
- Read: `I:\own\skills-visualstudio-extensibility\docs\superpowers\testing\2026-05-31-visual-studio-extensibility-red.md`
- Test: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`

- [ ] **Step 1: Create the GREEN notes file**

```markdown
# Visual Studio Extensibility Skill GREEN Notes

## Scenario results
- Scenario 1:
- Scenario 2:
- Scenario 3:

## Improvements over RED
- Routing improved:
- Sample mapping improved:
- Hybrid guidance improved:
- Preview caveats improved:

## Revisions still needed
- 
```

- [ ] **Step 2: Re-run the same three prompts with the skill available**

Use the same prompts from Task 1.

Expected: answers route to the correct reference area, use VisualStudio.Extensibility terminology, and mention hybrid or preview limitations where appropriate.

- [ ] **Step 3: Record exact improvements and remaining misses**

Use notes like:

```markdown
Scenario 2: Routed correctly to commands, referenced the official command sample, and warned that classic guidance should only be used if the requested capability is unavailable in the new framework.
```

- [ ] **Step 4: Tighten any weak wording in `SKILL.md` or references**

If a scenario still blurs framework boundaries or misses sample routing, update the relevant file immediately and rerun that scenario.

Expected: `Revisions still needed` is either empty or contains only minor wording tweaks already fixed.

### Task 8: Package the skill

**Files:**
- Read: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\SKILL.md`
- Read: `I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility\references\install-and-prereqs.md`
- Create: `I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill`
- Test: `I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill`

- [ ] **Step 1: Create the output directory**

Run:

```powershell
New-Item -ItemType Directory -Force -Path "I:\own\skills-visualstudio-extensibility\dist" | Out-Null
```

Expected: `I:\own\skills-visualstudio-extensibility\dist` exists.

- [ ] **Step 2: Package and validate the skill**

Run:

```powershell
python "C:\Users\mino\.copilot\installed-plugins\anthropic-agent-skills\document-skills\skills\skill-creator\scripts\package_skill.py" "I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility" "I:\own\skills-visualstudio-extensibility\dist"
```

Expected: validation passes and the file `I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill` is created.

- [ ] **Step 3: Verify the packaged file exists**

Run:

```powershell
Get-Item "I:\own\skills-visualstudio-extensibility\dist\developing-visual-studio-extensibility.skill"
```

Expected: PowerShell prints the file metadata without errors.

- [ ] **Step 4: Do a final package sanity check**

Run:

```powershell
Get-ChildItem "I:\own\skills-visualstudio-extensibility\developing-visual-studio-extensibility" -Recurse | Select-Object FullName
```

Expected: the packaged skill source contains only the final `SKILL.md` and six reference files.
