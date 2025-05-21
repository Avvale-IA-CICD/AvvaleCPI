**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains call activities with generic names such as "Step 1", "Step 2", "Step 3", "Set Headers", "Custom Status", and "Log Async Exception". These should be named more descriptively to improve readability and understanding of their function.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow sets standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType`.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow utilizes custom headers for logging such as `SAP_MessageProcessingLogCustomStatus`, also, other headers are used to control routing and retries `Step` and `SAP_DataStoreRetries`

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains values for `description`, `source`, and `target`.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ`. It should follow Java naming conventions (e.g., `com.example.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes several parameters such as `Data Store Name`, `MaxRetries`, `Poll Interval`, `RoleName` and other parameters related to DataStore Consumer adapter.

- **Error Handling** -> 🟢 Ok\
    The iFlow includes error handling using Error Event Subprocesses in processes "Step 1", "Step 2", "Step 3" and "SEDA Router".

- **Local Script Security** -> 🟢 Ok\
    The groovy scripts don´t use clases from these packages com.sap.it.api.securestore or com.sap.it.api.keystore

- **Iflow Organization** -> 🟢 Ok\
    The iFlow doesn't have more than 10 "callActivity" in the same "SequenceFlow"

- **Iflow Attachments** -> 🟢 Ok\
    The iFlow doesn´t use class messageLogFactory to create attachments

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow doesn't process IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iFlow doesn't process files directly.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iflow exposes endpoints with HTTPS Sender Adapter. The iFlow is configured to allow `ESBMessaging.send` role via externalized value. This require a double check in the ESBMessaging.send configuration to ensure that only allowed users can call this endpoint.