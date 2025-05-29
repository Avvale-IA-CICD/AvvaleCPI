markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains multiple "callActivity" elements with generic names like "Step 1", "Step 2", "Custom Status", "Set Headers".  These should be more descriptive.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow utilizes standard SAP headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType` and `SAP_MessageProcessingLogCustomStatus`.

- **Monitoring Custom Headers** -> 🔴 Check Required\
    The iFlow adds the `Step` header, but there is room for improvement in adding more custom headers to enhance payload search and filtering. Specifically, logging headers for input/output messages after key steps would aid in monitoring.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains `description`, `source`, and `target` metadata.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPS OL`.  It should use java notation (dots instead of underscores/hyphens), like `com.repsol.seda.model`.

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes parameters such as `Data Store Name`, `MaxRetries`, `Poll Interval`, `RoleName`, `Expiration Period` and others.

- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling using Error Subprocesses in several of the Integration Processes, including logging via `Log Async Exception`.  It also handles discarded messages after max retries.

- **Local Script Security** -> 🟢 Ok\
    The iFlow does not use classes from packages `com.sap.it.api.securestore` or `com.sap.it.api.keystore` in local scripts.

- **Iflow Organization** -> 🟢 Ok\
    No sequence flow uses more than 10 callActivities.

- **Iflow Attachments** -> 🟢 Ok\
    The iflow does not appear to be creating messageLogFactory attachments for successful messages in groovy scripting.

- **IDoc Rules** -> 🟡 Does not apply\
    The iflow does not process IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iflow does not process Files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an HTTPS endpoint (`/seda/start/ds`) and uses Role Based Authentication. The iFlow is configured to allow `ESBMessaging.send` role, which needs to be carefully evaluated in terms of security. Basic Auth is disabled, which is good.
