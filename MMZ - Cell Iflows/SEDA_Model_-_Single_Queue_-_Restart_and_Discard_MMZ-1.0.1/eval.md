**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
The iflow contains generic step names like "Step 1", "Step 2", "Step 3", "Next Step" and "Custom Status". More descriptive names should be used.

- **Monitoring Standard Headers** -> 🟢 Ok\
The iflow is using standard headers like SAP_Sender, SAP_Receiver, and SAP_MessageType.

- **Monitoring Custom Headers** -> 🟢 Ok\
The iflow is using custom headers SAP_MessageProcessingLogCustomStatus.

- **Iflow Metadata** -> 🟢 Ok\
The metainfo.prop file contains values for source, target and description.

- **Iflow Id** -> 🔴 Check Required\
The Bundle-SymbolicName contains hyphens. It should use java notation with dots instead. For example: com.example.seda.model

- **Parameter Externalization** -> 🟢 Ok\
The iflow is externalizing important parameters such as Queue Names, Expiration Period, Retry Interval, Number of Concurrent Processes and Retention Threshold 4 Alerting.

- **Error Handling** -> 🟢 Ok\
The iflow contains error handling using error sub processes in multiple Integration Processes ("Step 1 Exception Subprocess", "Step 2 Exception Subprocess", "Step 3 Exception Subprocess","Exception Subprocess 1","Trigger Exception Subproces").

- **Local Script Security** -> 🟢 Ok\
The iflow is not using classes from packages com.sap.it.api.securestore or com.sap.it.api.keystore in any of the scripts.

- **Iflow Organization** -> 🟢 Ok\
No SequenceFlow contains more than 10 call activities.

- **Iflow Attachments** -> 🔴 Check Required\
The iflow does not seem to be creating attachments, but it's important to confirm that the scripts don't use `messageLogFactory` in a way that could lead to excessive resource consumption.

- **IDoc Rules** -> 🟡 Does not apply\
This iflow is not related to IDocs.

- **File Rules** -> 🟡 Does not apply\
This iflow is not related to file processing.

- **Endpoint Rules** -> 🔴 Check Required\
The iFlow contains an HTTPS sender adapter ("Postman") exposing an endpoint. The userRole is set to `ESBMessaging.send` and `enableBasicAuthentication` is set to false, and the `senderAuthType` is set to `RoleBased`, which is considered the best practice.