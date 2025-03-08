# Key Component Changes
### Plugin Manager (plugin_manager.py)

- Discovers and loads plugins from directories
- Manages hook execution order
- Handles both packaged plugins and environment-based hooks

### Callback System

Three main callback types:

- create_job_bundle: Modify job bundles before submission
- ui: Customize UI and modify settings
- post_submit: Post-job actions

### Enhanced UI Components

- Modified tabs to support plugin overrides
- Added plugin widget injection points
- Dynamic UI element handling

## Sequence Diagram

The following diagram shows the sequence of events with the new PluginManager

```mermaid
sequenceDiagram
    participant D as Submit Dialog
    participant P as Plugin Manager
    participant H as Plugin Hook

    D->>P: Initialize with plugins directory
    P->>H: Load plugin classes
    D->>P: Create hook plugins
    P->>H: Register environment callbacks

    loop For each plugin
        D->>P: Call UI hook
        P->>H: on_ui_callback()
        H-->>D: Return modified settings/assets/UI
    end

    D->>User: Show enhanced UI

    When Submitting:
        D->>P: Call bundle hook
        P->>H: on_create_job_bundle()
        H-->>D: Modify bundle parameters

    After Submission:
        D->>P: Call post-submit hook
        P->>H: on_post_submit()
```
