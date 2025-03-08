# Arnold Plugin Integration with PluginManager
This Arnold plugin implements the `DeadlineCloudCallbackType` interface to integrate with the `PluginManager` system, extending Maya's submission dialog with Arnold-specific controls.


This integration pattern allows Arnold rendering capabilities to extend the base Maya submitter through the PluginManager system, while maintaining isolation from core submission logic.

____

## Integration Points
### Plugin Registration
- Auto discovered by the `PluginManager` by scanning the `maya_submitter` plugins directory
- Implements 3 core hooks:
```python
class ArnoldPlugin(DeadlineCloudCallbackType):
    def on_ui_callback(...) -> UICallbackResponse
    def on_create_job_bundle_callback(...)
    def on_post_submit_callback(...)
```

### UI Injection
`on_ui_callback` provices a custom QWidget
```python
return UICallbackResponse(job_specific_ui=ArnoldSubmitterPluginWidget(...))
```

### Job Bundle Customization
`on_create_job_bundle_callback`:
- Converts Maya scene to .ASS format
- Modifies job template parameters for Arnold

## Integration Diagram
```
sequenceDiagram
    participant Maya as Maya Submitter
    participant PM as PluginManager
    participant AP as ArnoldPlugin

    Maya->>PM: Initialize with plugins directory
    PM->>AP: Load ArnoldPlugin class
    Maya->>PM: Create submission dialog

    Maya->>PM: Build UI
    PM->>AP: Call on_ui_callback()
    AP-->>PM: Return Arnold settings widget
    PM-->>Maya: Render UI with Arnold section

    Maya->>User: Shows Arnold controls injected into Maya Submitter

    When Submitting:
        Maya->>PM: Prepare job bundle
        PM->>AP: Call on_create_job_bundle_callback()
        AP->>Maya: Convert scene to .ASS
        AP->>PM: Add Arnold parameters/assets
        PM-->>Maya: Final job bundle
     Maya->>Deadline Cloud: Submit .ASS job
```
