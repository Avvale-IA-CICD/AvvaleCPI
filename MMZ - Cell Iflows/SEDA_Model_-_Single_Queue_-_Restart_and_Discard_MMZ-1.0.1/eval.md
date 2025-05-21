**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains `callActivity` elements with default names such as "Step 1", "Step 2", "Step 3", "Custom Status", "Next Step", "Discaded", "Set Headers" and "Log Async Exception".

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow is using Standard Headers for monitoring such as SAP_Sender, SAP_Receiver, SAP_MessageType and SAP_MessageProcessingLogCustomStatus.

- **Monitoring Custom Headers** -> 🔴 Check Required\
    The iFlow only logs custom status. Adding custom header for payload search and filtering may improve monitoring.

- **Iflow Metadata** -> 🟢 Ok\
    The metainfo.prop file contains values for source, target and description.

- **Iflow Id** -> 🔴 Check Required\
    The Bundle-SymbolicName in MANIFEST.MF (SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ) does not follow java notation conventions. It should use dots and not hyphens or underscores, example: `com.sap.example.seda`.

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes important parameters such as queue names, retry intervals, etc.

- **Error Handling** -> 🟢 Ok\
    The iFlow contains error sub-processes with logging of async exceptions.

- **Local Script Security** -> 🟢 Ok\
    The iFlow does not use classes from com.sap.it.api.securestore or com.sap.it.api.keystore.

- **Iflow Organization** -> 🟢 Ok\
    The iFlow does not have more than 10 `callActivity` elements in the same `SequenceFlow`.

- **Iflow Attachments** -> 🔴 Check Required\
    The iFlow is not creating attachments for succesful messages

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process files.

- **Endpoint Rules** -> 🔴 Check Required\
    The iflow exposes an endpoint with HTTPS Sender Adapter and uses ESBMessaging.send role, either hardcoded in BPMN XML or in Configured values, that´s considered insecure.