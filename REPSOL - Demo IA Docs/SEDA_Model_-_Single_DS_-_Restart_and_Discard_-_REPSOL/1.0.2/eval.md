**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.2

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iflow contains several "callActivity" steps with generic names like "Step 1", "Step 2", "Set Headers", "Custom Status" and "Log Async Exception". These should be renamed to reflect their specific function within the iFlow.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard SAP headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` in the "Set Headers" steps for monitoring purposes. `SAP_MessageProcessingLogCustomStatus` is also being used which is a good sign.

- **Monitoring Custom Headers** -> 🔴 Check Required\
    The iflow could benefit from logging additional custom headers for enhanced payload search and filtering. There are few `SAP_MessageProcessingLogCustomStatus`, but it´s necesary to determine if those are enough for good monitoring, so review is recomended.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains values for `description`, `source`, and `target`, indicating that the iFlow metadata is populated.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_-_REPSOL`. It should use a Java-style notation with dots (e.g., `com.repsol.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes several parameters including retry intervals, data store name, role name, expiration period, lock timeout and alerting thresholds.

- **Error Handling** -> 🟢 Ok\
    The iflow implements error handling using error sub-processes in each "Step" (Step 1, Step 2, Step 3, and the SEDA Router process), logging exceptions using the "Log Async Exception" process.

- **Local Script Security** -> 🔴 Check Required\
    The iflow contains groovy scripts, so review is required to determine if any local script is using classes from `com.sap.it.api.securestore` or `com.sap.it.api.keystore`, which may represent a security issue. The script `script1.groovy` is used in "Test Throw Exception" call activity and scripts `Log_Discarded_Message.groovy` and `Log_Exception_Async.groovy` are used in  "Log Discarded Message" and "Log Async Exception" call activities respectively.

- **Iflow Organization** -> 🟢 Ok\
    The iFlow doesn't appear to have more than 10 "callActivity" elements in the same "SequenceFlow".

- **Iflow Attachments** -> 🔴 Check Required\
    The iflow contains groovy scripts, so review is required to determine if any local script is creating attachments for successful messages, making use of class messageLogFactory. Scripts `script1.groovy`, `Log_Discarded_Message.groovy` and `Log_Exception_Async.groovy` should be reviewed.

- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process IDoc messages.

- **File Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process files directly.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an HTTPS endpoint. It's configured to use RoleBased authentication with the role `ESBMessaging.send`. Verify that basic authentication is disabled, and that `ESBMessaging.send` is appropriate and secure for this endpoint, the RoleName property is configured as "{{RoleName}}", check value on configuration.