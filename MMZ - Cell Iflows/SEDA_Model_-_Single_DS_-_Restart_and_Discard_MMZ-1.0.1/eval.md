markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    Several call activities have generic names like "Step 1", "Step 2", "Custom Status" and "Set Headers".

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, and `SAP_MessageProcessingLogCustomStatus`.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow leverages SAP_MessageProcessingLogCustomStatus header

- **Iflow Metadata** -> 🟢 Ok\
    The metainfo.prop file contains values for source, target and description.

- **Iflow Id** -> 🔴 Check Required\
    The Bundle-SymbolicName in MANIFEST.MF uses hyphens and underscores (`SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ`).  It should use Java-style notation with dots (e.g., `com.example.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
    Several parameters are externalized as properties (e.g., Data Store Name, Poll Interval, MaxRetries, RoleName, etc.).

- **Error Handling** -> 🟢 Ok\
    The iFlow uses error sub-processes and calls a "Log Async Exception" process to handle errors in multiple steps.

- **Local Script Security** -> 🔴 Check Required\
    The iFlow utilizes groovy scripting, but doesn't seem to use "com.sap.it.api.securestore" or "com.sap.it.api.keystore", but the existance of groovy scripts should be revised for security purposes.

- **Iflow Organization** -> 🟢 Ok\
    No sequence flow has more than 10 call activities.

- **Iflow Attachments** -> 🔴 Check Required\
    The iFlow utilizes groovy scripting, it's required to check the content of "Log_Discarded_Message.groovy" and "Log_Exception_Async.groovy" for messageLogFactory class usage.

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow doesn't seem to be processing IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iFlow doesn't seem to be processing files.

- **Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with HTTPS Sender Adapter. The current setup relies on RoleBased authentication. Review the {{RoleName}} parameter and related authorization configuration to assure it follows security best practices.
