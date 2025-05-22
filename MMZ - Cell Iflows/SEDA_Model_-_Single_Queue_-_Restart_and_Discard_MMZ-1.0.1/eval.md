**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
The iFlow contains call activities with generic names such as "Step 1", "Step 2", "Step 3", "Set Headers" and "Custom Status". While these names might be descriptive in this specific context, it's generally advisable to use more specific names that clearly indicate the purpose of each step.

- **Monitoring Standard Headers** -> 🟢 Ok\
The iFlow uses standard SAP headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` in the "Set Headers" call activities of the "Dummy Start" and "SEDA Router" processes.

- **Monitoring Custom Headers** -> 🔴 Check Required\
The iFlow uses custom headers for setting custom status (`SAP_MessageProcessingLogCustomStatus`) to improve logging and monitoring, but this could be improved with other values that enhance payload search and filtering.

- **Iflow Metadata** -> 🟢 Ok\
The `metainfo.prop` file contains values for source, target and description.

- **Iflow Id** -> 🔴 Check Required\
The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ`. It should follow Java package naming conventions (e.g., `com.example.seda.model`). Using underscores and hyphens is not recommended.

- **Parameter Externalization** -> 🟢 Ok\
The iFlow externalizes parameters like queue names, retry intervals, expiration periods, and maximum retries in the configuration parameters.

- **Error Handling** -> 🟢 Ok\
The iFlow contains exception subprocesses within "Step 1", "Step 2", "Step 3" and "SEDA Router" processes, and includes a "Trigger Exception Subprocess" in "Dummy Start". It logs exceptions using a dedicated "Log Async Exception" process, defines custom statuses and sets error end events.

- **Local Script Security** -> 🟢 Ok\
The provided code doesn't use `com.sap.it.api.securestore` or `com.sap.it.api.keystore` classes.

- **Iflow Organization** -> 🟢 Ok\
The iFlow does not have more than 10 "callActivity" in any "SequenceFlow".

- **Iflow Attachments** -> 🟢 Ok\
The iFlow does not create attachments for successful messages.

- **IDoc Rules** -> 🟡 Does not apply\
The iFlow doesn't process IDoc messages.

- **File Rules** -> 🟡 Does not apply\
The iFlow doesn't process files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
The iFlow exposes an endpoint with HTTPS Sender Adapter, configured to allow `ESBMessaging.send` role. Check if the iflow is not configured to allow Basic Auth. This is considered insecure.