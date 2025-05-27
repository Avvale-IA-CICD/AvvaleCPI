**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.2

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains `callActivity` elements with default names such as "Step 1", "Step 2", "Step 3", "Custom Status", "Set Headers", "Discaded", "Log Async Exception" and "Prepare Step 2".  These should be renamed to reflect their specific function.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` in the "Set Headers" `callActivity` elements.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow uses custom header like `Step` in the "Set Headers" and "Prepare Step 2" `callActivity` elements. SAP_MessageProcessingLogCustomStatus is being used with expression values.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains values for `description`, `source`, and `target`.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL`. It should follow java notation using dots, not underscores or hyphens (e.g., `com.repsol.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes parameters such as `Data Store Name`, `RoleName`, `Expiration Period`, `MaxRetries`, `Retry Interval`, `Poll Interval`, `Lock Timeout`, `Maximum Retry Interval`, `Exponential Backoff`, `Retention Threshold 4 Alerting` and queue name `SEDA_MAIN_QUEUE`.

- **Error Handling** -> 🟢 Ok\
    The iFlow uses Error Event Subprocesses in processes "Step 1", "Step 2", "Step 3", "SEDA Router", "Trigger Exception Subprocess" including logging via the "Log Async Exception" process. Additionally in "SEDA Router" iflow, is discarding messages based on retries count and logging discarded messages. "Step 3" iflow is throwing a dummy exception.

- **Local Script Security** -> 🔴 Check Required\
    The iFlow uses groovy scripts without classes from `com.sap.it.api.securestore` or `com.sap.it.api.keystore`. One of the scripts is used to create a dummy exception. The script "Log_Discarded_Message.groovy" and "Log_Exception_Async.groovy" needs to be verified.

- **Iflow Organization** -> 🟢 Ok\
    No "SequenceFlow" contains more than 10 "callActivity".

- **Iflow Attachments** -> 🔴 Check Required\
    The iflow "Log Async Exception" makes use of `Groovy_Logging_Scripts` to log Async Exceptions. Only failed messages should be logged, normally in exception process. It needs to be verified.

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with HTTPS Sender Adapter ("Postman") and uses `ESBMessaging.send` role externalized in Config parameters section. Check if Basic Auth is disabled (enableBasicAuthentication = false)