**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
The iflow contains `callActivity` elements named "Step 1", "Step 2", "Step 3", "Set Headers", "Custom Status", "Log Async Exception", "Log Discarded Message", "Discaded" which are considered not descriptive and should be renamed for better readability.

- **Monitoring Standard Headers** -> 🟢 Ok\
The iflow uses standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` in the "Dummy Start" process and in "SEDA Router" process.

- **Monitoring Custom Headers** -> 🔴 Check Required\
There is usage of custom headers like "Step", but the purpose is not well defined, consider adding more explicit custom headers that enhance payload search and filtering.

- **Iflow Metadata** -> 🟢 Ok\
The `metainfo.prop` file contains values for source, target, and description.

- **Iflow Id** -> 🔴 Check Required\
The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ`. It should follow Java package naming conventions (e.g., `com.example.seda.model`).

- **Parameter Externalization** -> 🟢 Ok\
The iflow externalizes parameters such as `MaxRetries`, `Data Store Name`, `RoleName`, `Expiration Period`, `Lock Timeout`, `Maximum Retry Interval`, and `Poll Interval`, Authentication data, URLs and Location ID are not present as parameters.

- **Error Handling** -> 🟢 Ok\
The iflow includes error handling using Error Subprocesses in processes "Step 1", "Step 2", "Step 3", "Dummy Start" and "SEDA Router". These processes include logging exceptions using a Groovy script ("Log_Exception_Async.groovy").

- **Local Script Security** -> 🟢 Ok\
The iflow does not use classes from the `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages in the provided groovy scripts.

- **Iflow Organization** -> 🟢 Ok\
The iflow does not have more than 10 `callActivity` elements within the same `SequenceFlow`.

- **Iflow Attachments** -> 🔴 Check Required\
The iflow does not use class `messageLogFactory` to create attachments.

- **IDoc Rules** -> 🟡 Does not apply\
The iflow does not process IDocs.

- **File Rules** -> 🟡 Does not apply\
The iflow does not process Files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
The iFlow uses HTTPS sender adapter and is configured to allow Role Based Authentication with `ESBMessaging.send` role, that´s considered insecure.