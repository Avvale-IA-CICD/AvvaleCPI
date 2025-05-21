**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

-

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
        The iFlow contains `callActivity` elements with generic names like "Step 1", "Step 2", "Set Headers" and "Custom Status". These could be more descriptive.
    - **Monitoring Standard Headers** -> 🟢 Ok\
        The iFlow uses standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType`.
    - **Monitoring Custom Headers** -> 🟢 Ok\
        The iFlow uses custom headers such as `Step` and creates custom statuses using `SAP_MessageProcessingLogCustomStatus`.
    - **Iflow Metadata** -> 🟢 Ok\
        The metainfo.prop file is populated with source, target, and description.
    - **Iflow Id** -> 🔴 Check Required\
        The Bundle-SymbolicName `SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ` uses underscores and hyphens. It should follow Java package naming conventions (e.g., `com.example.seda`).
    - **Parameter Externalization** -> 🟢 Ok\
        The iFlow externalizes important parameters such as `Data Store Name`, `RoleName`, `MaxRetries`, `Poll Interval`, `Retry Interval`, `Expiration Period`, `Lock Timeout` etc.
    - **Error Handling** -> 🟢 Ok\
        The iFlow includes error sub-processes for exception handling in most integration processes (Step 1, Step 2, Step 3, SEDA Router, Dummy Start), logging exceptions using `Log Async Exception`.
    - **Local Script Security** -> 🟢 Ok\
        The iFlow does not appear to use classes from `com.sap.it.api.securestore` or `com.sap.it.api.keystore`.
    - **Iflow Organization** -> 🟢 Ok\
        The iFlow does not have sequence flows with more than 10 call activities.
    - **Iflow Attachments** -> 🟢 Ok\
        The iFlow does not appear to create attachments using `messageLogFactory` during groovy scripting.
    - **IDoc Rules** -> 🟡 Does not apply\
        The iFlow does not appear to process IDocs.
    - **File Rules** -> 🟡 Does not apply\
        The iFlow does not appear to process Files.
    - **Endpoint Rules** -> 🔴 Check Required\
        The iFlow exposes an endpoint with HTTPS Sender Adapter and is configured to allow Role Based authentication with `ESBMessaging.send` role.