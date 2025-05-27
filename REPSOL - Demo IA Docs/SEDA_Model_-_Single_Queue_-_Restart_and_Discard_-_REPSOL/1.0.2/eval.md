markdown
**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.2

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    - The iflow contains several "callActivity" elements with generic names such as "Step 1", "Step 2", "Step 3", "Log Async Exception" and "Set Headers". While the names are descriptive, it's always best to add more context about what the steps actually do.

- **Monitoring Standard Headers** -> 🟢 Ok\
    - The iflow is using standard headers such as `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` for monitoring purposes.

- **Monitoring Custom Headers** -> 🟢 Ok\
    - The iflow uses `SAP_MessageProcessingLogCustomStatus` which can be considered a custom header for monitoring purposes.

- **Iflow Metadata** -> 🟢 Ok\
    - The `metainfo.prop` file contains `description`, `source`, and `target` metadata.

- **Iflow Id** -> 🔴 Check Required\
    - The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_Queue_-_Restart_and_Discard_-_R EPSOL`. This contains hyphens and underscores. It should follow Java notation (e.g., `com.repsol.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
    - The iflow externalizes parameters like queue names (`SEDA_MAIN_QUEUE`), retry intervals, and retry count (`MaxRetries`) in the configuration parameters.

- **Error Handling** -> 🟢 Ok\
    - The iflow contains exception sub-processes in multiple integration processes (Step 1, Step 2, Step 3 and SEDA Router) to handle errors. Additionally, there are Log Async Exception processes invoked on error, and custom status setting logic.

- **Local Script Security** -> 🔴 Check Required\
    - The iflow uses Groovy scripts (e.g., `Log_Discarded_Message.groovy`, `Log_Exception_Async.groovy`, `script1.groovy`). A manual check is required to verify if these scripts use classes from the `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages.

- **Iflow Organization** -> 🟢 Ok\
    - No `SequenceFlow` contains more than 10 "callActivity" elements.

- **Iflow Attachments** -> 🔴 Check Required\
    - The iflow has groovy script `Log_Exception_Async.groovy`, verify if this script is making use of class messageLogFactory to create attachments for successful messages, if positive then it represent a potential security/resource consumption issue.

- **IDoc Rules** -> 🟡 Does not apply\
    - There are no IDoc related steps in the iflow.

- **File Rules** -> 🟡 Does not apply\
    - There are no File adapter steps in the iflow.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    - The iFlow exposes an endpoint with HTTPS Sender Adapter, and uses ESBMessaging.send role, it´s necessary a manual check if Basic Auth is not configured for the Sender Adapter (senderAuthType is RoleBased, so it seems ok).
