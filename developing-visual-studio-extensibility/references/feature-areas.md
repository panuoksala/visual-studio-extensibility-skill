# Feature areas

## Commands

- **What this area is for:** Adding commands to VS menus, toolbars, and keyboard shortcuts using
  code-based `CommandConfiguration`. The `Command` base class replaces `.vsct` files entirely.
  Command logic is implemented by overriding `ExecuteCommandAsync`, which is the primary async
  entry point invoked when the user triggers the command.
- **When to route here:** The user wants to create a menu item, toolbar button, or keyboard
  shortcut; asks about command placement, icon, tooltip, or visibility constraints; or mentions
  `ExecuteCommandAsync`, `CommandConfiguration`, or `CommandPlacement`.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/command/command`
- **Sample-backed starting points:** Use `SimpleRemoteCommandSample` for the most direct command baseline, `CommandParentingSample` when placement or parenting is the main question, and `InsertGuid` when the command also edits the active document.
- **Common limitation or caveat:** Placement is limited to typed `CommandPlacement.KnownPlacements`
  locations (`ExtensionsMenu`, `ToolsMenu`, `ViewOtherWindowsMenu`, etc.). Arbitrary menu
  injection requires in-proc VSSDK. Activation constraints (`EnabledWhen` / `VisibleWhen`) are
  declared via `ActivationConstraint` expressions — not context GUIDs.

## Editor and documents

- **What this area is for:** Reading and writing text in the active document, subscribing to
  editor events, creating editor margins, tagger/classifier contributions, and code-lens entries.
- **When to route here:** The user wants to insert, replace, or read text; create an editor
  margin or adornment; listen to caret or selection changes; or access `ITextDocumentSnapshot`.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/editor/editor`
- **Sample-backed starting points:** Use `InsertGuid` for command-triggered text edits, `DocumentSelectorSample` when applicability depends on file path matching, `WordCountMargin` for editor margins, and `MarkdownLinter` when the scenario spans multiple editor-facing components.
- **Common limitation or caveat:** Not all editor APIs have VisualStudio.Extensibility equivalents
  yet; advanced editor scenarios (e.g., full language service integration) typically require the
  hybrid path. Route to `hybrid-and-migration.md` when coverage is incomplete.

## Output window

- **What this area is for:** Writing diagnostic or status messages to a named output pane in the
  VS Output window via `IOutputWindowExtensibility`.
- **When to route here:** The user wants to create an output channel, write log lines, or display
  progress text in the Output window.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/output-window/output-window`
- **Sample-backed starting points:** Use `OutputWindowSample` for the basic output-pane flow, and `MarkdownLinter` when the user needs a broader example that includes output-window reporting alongside other extension components.
- **Common limitation or caveat:** Extensions create their own named channel; they cannot write
  to VS-owned channels (Build, Debug, etc.). There is no rich-text or color support beyond plain
  text in the current API.

## Tool windows

- **What this area is for:** Creating custom dockable panels in VS. The view runs in the VS
  process using Remote UI; the view-model runs in the extension process.
- **When to route here:** The user wants a custom panel, dockable side window, or persistent UI
  surface inside VS; mentions `ToolWindow`, `RemoteUserControl`, or `IToolWindowService`.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/tool-window/tool-window`
- **Common limitation or caveat:** WPF resources and converters must be declared using the Remote
  UI pattern (no direct WPF resource dictionary injection). The XAML view and the C# view-model
  live in different processes; cross-process serialization constraints apply to all bound data.

## User prompts and dialogs

- **What this area is for:** Showing simple yes/no/cancel confirmation prompts via
  `IUserPromptService`, and custom modal dialogs with Remote UI content.
- **When to route here:** The user wants to ask a yes/no question, display a message box, or
  build a form-like dialog inside VS.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/user-prompt/user-prompts`
  and `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/dialog/dialog`
- **Common limitation or caveat:** Do not call `MessageBox.Show` or
  `ThreadHelper.JoinableTaskFactory.Run` from extension code. Custom dialogs are Remote UI and
  subject to the same cross-process data constraints as tool windows.

## Debugger visualizers

- **What this area is for:** Providing custom visualizers shown in the VS debugger for specific
  .NET types, using `DebuggerVisualizerProvider`.
- **When to route here:** The user wants to display a custom UI for a type's value when it
  appears in the Watch window, DataTips, or debugger variable panes.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/debugger-visualizer/debugger-visualizers`
- **Common limitation or caveat:** VisualStudio.Extensibility visualizers are class-based
  (`DebuggerVisualizerProvider`), not attribute-based (`[DebuggerTypeProxy]`). The visualizer
  UI is Remote UI running out-of-process; the debuggee object must be serializable across the
  process boundary.

## Project query

- **What this area is for:** Querying project structure — source files, references, build
  configurations, project properties — using the VS Project Query API
  (`IProjectModelQueryableSpace`).
- **When to route here:** The user wants to enumerate projects or solution items, read file
  lists, inspect build configurations, or react to project changes.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/project/project`
- **Common limitation or caveat:** The default query surface is read-oriented. Mutations (adding
  files, changing properties) require calling methods on the mutable project interfaces, and not
  all project system types expose full mutation support. Complex project-system operations may
  still require falling back to IVsSolution or IVsProject from VSSDK.

## Settings

- **What this area is for:** Declaring, persisting, and reading user-scoped settings for an
  extension using `ISettingsManager` and settings contribution classes.
- **When to route here:** The user wants to add a settings page in VS Options, store user
  preferences, or read a persisted value at runtime.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/settings/settings`
- **Common limitation or caveat:** Settings are declared as contributions (`[VisualStudioContribution]`)
  and are distinct from the classic VSSDK `IVsSettingsManager` / registry-based store. Do not
  mix the two; using classic registry settings in an out-of-proc extension is not supported
  without the hybrid template.

## Language Server Provider

- **What this area is for:** Hosting an LSP-compliant language server from within the extension
  to provide language features (completion, hover, diagnostics, go-to-definition) for a custom
  language.
- **When to route here:** The user wants to add language-intelligence features for a language
  that VS does not natively support, or mentions LSP, `LanguageServerProvider`, or
  `ILanguageServerManager`.
- **Official Learn article to consult first:**
  `learn.microsoft.com/visualstudio/extensibility/visualstudio.extensibility/language-server-provider/language-server-provider`
- **Common limitation or caveat:** The Language Server Provider API is marked `[Experimental]`
  and subject to breaking changes. Client-capability negotiation and some LSP message types may
  behave differently from other editors. Verify against the breaking-changes page before
  committing to this approach:
  `github.com/microsoft/VSExtensibility/blob/main/docs/breaking_changes.md`
