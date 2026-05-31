# Samples and patterns

| Goal | Start here |
| --- | --- |
| Add a command | SimpleRemoteCommandSample, CommandParentingSample |
| Add a command that also writes to the editor | InsertGuid |
| Add editor text manipulation | InsertGuid |
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

## Pattern notes

- Prefer the official sample that matches the user's immediate goal instead of combining multiple samples too early.
- Mention contribution placement and activation constraints when they affect commands or UI.
- Use migration or hybrid guidance when the sample requires classic SDK integration.
