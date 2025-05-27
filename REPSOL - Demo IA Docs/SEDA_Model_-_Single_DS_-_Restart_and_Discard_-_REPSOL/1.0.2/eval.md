**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.2

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
The iFlow contains "callActivity" steps with generic names like "Step 1", "Step 2", "Step 3", "Custom Status", "Set Headers" and Log Async Exception". These should be renamed to reflect their specific function within the iFlow.

- **Monitoring Standard Headers** -> 🟢 Ok\
The iFlow uses standard headers like SAP_Sender, SAP_Receiver and SAP_MessageType.

- **Monitoring Custom Headers** -> 🟢 Ok\
The iFlow uses custom headers like Step to control routing. And the Custom Status steps are populating SAP_MessageProcessingLogCustomStatus.

- **Iflow Metadata** -> 🟢 Ok\
The file metainfo.prop is populated with source, target, and description.

- **Iflow Id** -> 🔴 Check Required\
The Bundle-SymbolicName in MANIFEST.MF is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL`. This should follow Java notation and use dots instead of underscores or hyphens. A valid example is com.sap.example.seda.

- **Parameter Externalization** -> 🟢 Ok\
The iFlow externalizes important parameters such as `Data Store Name`, `RoleName`, `MaxRetries`, `Expiration Period` and intervals.

- **Error Handling** -> 🟢 Ok\
The iFlow implements error handling using error sub-processes in multiple integration processes (`Step 1`, `Step 2`, `Step 3`, `SEDA Router`, `Trigger Exception Subprocess`). It also includes a dedicated process `Log Async Exception` to log errors. The main process retries and discards messages after a certain number of retries.

- **Local Script Security** -> 🔴 Check Required\
The iFlow uses a local groovy script `script1.groovy` in the `Test Throw Exception` step of the Process_44 (`Step 3`). Review the scripts' content and ensure no usage from these packages: `com.sap.it.api.securestore` or `com.sap.it.api.keystore` is done.

- **Iflow Organization** -> 🟢 Ok\
The iFlow does not have more than 10 "callActivity" elements within a single "SequenceFlow."

- **Iflow Attachments** -> 🔴 Check Required\
The iFlow uses groovy script `Log_Discarded_Message.groovy` and `Log_Exception_Async.groovy` in the `SEDA Router` and Exception sub-processes. Review those scripts' content and ensure it is not creating attachments using `messageLogFactory` for *successful* messages. Attachment creation is acceptable when dealing with exception process, logging only failed messages.

- **IDoc Rules** -> 🟡 Does not apply\
The iFlow does not appear to process IDocs.

- **File Rules** -> 🟡 Does not apply\
The iFlow does not appear to process files directly.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
The iFlow exposes an endpoint with HTTPS Sender Adapter. The iFlow is configured to use RoleBased authentication with the role `ESBMessaging.send`. While RoleBased is preferable to Basic Authentication, using the `ESBMessaging.send` role is considered insecure and a more specific custom role should be created.