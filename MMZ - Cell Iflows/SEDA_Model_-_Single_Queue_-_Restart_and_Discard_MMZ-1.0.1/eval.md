markdown
**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
   The iFlow contains "callActivity" elements named "Sender", "Receiver", "Content Modifier", "Request Reply", or "Router" like names that are part of default generated elements. Specifically, "Set Headers" and "Custom Status" are used which are not descriptive enough.

- **Monitoring Standard Headers** -> 🟢 Ok\
   The iFlow uses standard headers for monitoring, such as `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType`.

- **Monitoring Custom Headers** -> 🟢 Ok\
   The iFlow uses custom status codes by populating  `SAP_MessageProcessingLogCustomStatus`.

- **Iflow Metadata** -> 🟢 Ok\
   The `metainfo.prop` file is present and contains values for source, target, and description.

- **Iflow Id** -> 🔴 Check Required\
   The `Bundle-SymbolicName` in `MANIFEST.MF` (`SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ`) uses hyphens and underscores. Java notation using dots is recommended. Example: `com.sap.example.seda`.

- **Parameter Externalization** -> 🟢 Ok\
   The iFlow externalizes parameters such as queue names (`SEDA_MAIN_QUEUE`), retry intervals (`Retry Interval`, `Maximum Retry Interval`), and retention thresholds (`Retention Threshold 4 Alerting`, `Expiration Period`).

- **Error Handling** -> 🟢 Ok\
   The iFlow implements error handling using error subprocesses in the main process as well as in called processes for each step. It also includes exception logging.

- **Local Script Security** -> 🟢 Ok\
   The iFlow does not use classes from the `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages in local scripts.

- **Iflow Organization** -> 🟢 Ok\
   No `SequenceFlow` has more than 10 "callActivity" elements.

- **Iflow Attachments** -> 🟢 Ok\
   The iFlow does not create attachments for successful messages using `messageLogFactory`.

- **IDoc Rules** -> 🟡 Does not apply\
   The iFlow does not handle IDoc messages.

- **File Rules** -> 🟡 Does not apply\
   The iFlow does not process any files.

- **Endpoint Rules** -> 🔴 Check Required\
   The iFlow exposes an endpoint with an HTTPS sender adapter and is configured to allow Basic Auth with ESBMessaging.send, that´s considered insecure.
