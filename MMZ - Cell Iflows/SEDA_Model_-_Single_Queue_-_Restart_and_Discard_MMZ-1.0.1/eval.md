**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains generic `callActivity` names such as "Step 1", "Step 2", "Step 3" and "Set Headers" which should be more descriptive.
- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers like SAP_Sender, SAP_Receiver, and SAP_MessageType for monitoring.
- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow uses custom headers such as SAP_MessageProcessingLogCustomStatus for enhancing payload search and filtering.
- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains the `description`, `source`, and `target` metadata values for the iFlow.
- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ`. It should follow Java notation (e.g., `com.example.seda.model`).
- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes important parameters like queue names, retry intervals, and expiration periods, using placeholders like `{{SEDA_MAIN_QUEUE}}`, `{{MaxRetries}}` and `{{Expiration Period}}`.
- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling using Error Subprocesses and logging exceptions.
- **Local Script Security** -> 🔴 Check Required\
    The iFlow contains Groovy scripts. Further analysis is needed to check for usage of classes `com.sap.it.api.securestore` or `com.sap.it.api.keystore` inside groovy scripts.
- **Iflow Organization** -> 🟢 Ok\
    The iFlow doesn't seem to have more than 10 `callActivity` in the same `SequenceFlow`.
- **Iflow Attachments** -> 🔴 Check Required\
    The Groovy script `Log_Exception_Async.groovy` requires further investigation to determine whether it uses `messageLogFactory` to create attachments for successful messages, as this might represent a security or resource consumption issue.
- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow doesn't appear to process IDocs.
- **File Rules** -> 🟡 Does not apply\
    The iFlow doesn't appear to process files directly.
- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an HTTPS endpoint and utilizes the `ESBMessaging.send` role which requires a check that no basic authentication is allowed.