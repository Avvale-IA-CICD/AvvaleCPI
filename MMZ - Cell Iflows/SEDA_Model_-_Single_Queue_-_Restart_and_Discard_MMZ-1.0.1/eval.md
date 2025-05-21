**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
The iflow contains "callActivity" steps with standard names such as "Custom Status", "Set Headers", "Next Step", "Discaded" and "Log Async Exception".

- **Monitoring Standard Headers** -> 🟢 Ok\
The iflow utilizes standard headers like SAP_Sender, SAP_Receiver and SAP_MessageType in multiple "callActivity" steps.

- **Monitoring Custom Headers** -> 🟢 Ok\
The iflow utilizes custom statuses for monitoring using the SAP_MessageProcessingLogCustomStatus header.

- **Iflow Metadata** -> 🟢 Ok\
The `metainfo.prop` file contains values for source, target and description.

- **Iflow Id** -> 🔴 Check Required\
The Bundle-SymbolicName in `MANIFEST.MF` (SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ) uses underscores and hyphens, which is not the recommended java notation.

- **Parameter Externalization** -> 🟢 Ok\
The iflow externalizes several important parameters such as queue names and retry intervals using placeholders like `{{SEDA_MAIN_QUEUE}}`, `{{Number of Concurrent Processes}}`, `{{Maximum Retry Interval}}`, `{{Retry Interval}}`, `{{Retention Threshold 4 Alerting}}`, `{{Expiration Period}}` and `{{MaxRetries}}`.

- **Error Handling** -> 🟢 Ok\
The iflow implements error handling using error sub-processes in several integration processes (Step 1, Step 2, Step 3 and SEDA Router). These sub-processes log exceptions and set custom statuses.

- **Local Script Security** -> 🟢 Ok\
The iflow does not appear to be using classes from `com.sap.it.api.securestore` or `com.sap.it.api.keystore`.

- **Iflow Organization** -> 🟢 Ok\
The iflow does not use more than 10 "callActivity" in any single "SequenceFlow".

- **Iflow Attachments** -> 🟢 Ok\
The provided content doesn't indicate that the developer is creating attachments for successful messages using `messageLogFactory`.

- **IDoc Rules** -> 🟡 Does not apply\
The iflow does not appear to be processing IDocs, so this rule does not apply.

- **File Rules** -> 🟡 Does not apply\
The iflow does not appear to be processing files, so this rule does not apply.

- **Endpoint Rules** -> 🔴 Check Required\
The iflow exposes an endpoint with HTTPS Sender Adapter (`Participant_12079805`) and `senderAuthType` is configured with value `RoleBased` and `userRole` with `ESBMessaging.send`. It requires a check if the `enableBasicAuthentication` property is set to `true` and if not, it would be considered a security issue.