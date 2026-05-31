# Samples and patterns

| Goal | Start here |
| --- | --- |
| Add a command | SimpleRemoteCommandSample, CommandParentingSample |
| Add a command that also writes to the editor | InsertGuid |
| Add editor text manipulation | InsertGuid |
| Add a command and control where it appears | CommandParentingSample |
| Add an output pane | OutputWindowSample |
| Limit an editor extension to matching files | DocumentSelectorSample |
| Combine multiple extensibility areas in one sample | MarkdownLinter |
| Consume classic SDK services through dependency injection | CommentRemover (in-proc / hybrid only) |
| Add a tool window | ToolWindowSample |
| Show a prompt | UserPromptSample |
| Show a dialog | DialogSample |
| Build a debugger visualizer | RegexMatchDebugVisualizer or MemoryStreamDebugVisualizer |
| Query project data | VSProjectQueryAPISample |
| Add an editor margin or adornment | WordCountMargin |
| Add a language server provider | RustLanguageServerProvider |
| Combine new and classic models | CompositeExtension |

All samples are in the `microsoft/VSExtensibility` GitHub repository under
`New_Extensibility_Model/Samples/`.

## Official sample catalog from Learn

Microsoft Learn's `About VisualStudio.Extensibility (Preview)` page is the discovery hub for the official sample set. Use it when the curated table above does not cover the user's scenario cleanly.

The full sample solution is `Samples.sln` in the `microsoft/VSExtensibility` repository under `New_Extensibility_Model/Samples/`.

Treat the table above as a routing layer for the most common capabilities, not as an exhaustive mirror of every sample in the repo.

## Pattern notes

- Prefer the official sample that matches the user's immediate goal instead of combining multiple samples too early.
- Mention contribution placement and activation constraints when they affect commands or UI.
- Use migration or hybrid guidance when the sample requires classic SDK integration.
