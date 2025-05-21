**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🟢 Ok\
    The iFlow steps have meaningful names (e.g., Step 1, Prepare Step 2, Log Async Exception). No default names like "Sender", "Receiver", "Content Modifier", etc., are present.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers for monitoring, including `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType`.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow uses custom headers for monitoring such as `SAP_MessageProcessingLogCustomStatus` for providing more context on the message processing. Also, `Step` property is used as Sender in exception flow.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains `description`, `source`, and `target` metadata.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ`. This does not follow the recommended Java package naming convention (e.g., `com.sap.something.something`). It should use dots instead of underscores and hyphens.

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes important parameters like queue names (`SEDA_MAIN_QUEUE`), retry intervals, and maximum retries.

- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling using error subprocesses, custom statuses, and logging of exceptions in several processes and subprocesses.

- **Local Script Security** -> 🟢 Ok\
    The iFlow does not use classes from the `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages in the provided script.

- **Iflow Organization** -> 🟢 Ok\
    There are no sequences with more than 10 "callActivity" in the same "SequenceFlow".

- **Iflow Attachments** -> 🟢 Ok\
    The iflow exception subprocess is calling the "Log Async Exception" process that should be making use of class messageLogFactory during groovy scripting. The usage is in exception process only so this does not represent a security/resource consumption issue.

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow doesn't appear to handle IDocs, so this rule doesn't apply.

- **File Rules** -> 🟡 Does not apply\
    The iFlow doesn't appear to handle files, so this rule doesn't apply.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with HTTPS Sender Adapter and is configured to allow RoleBased sender authentication. Since the userRole `ESBMessaging.send` is hardcoded in the BPMN XML, that´s considered insecure.