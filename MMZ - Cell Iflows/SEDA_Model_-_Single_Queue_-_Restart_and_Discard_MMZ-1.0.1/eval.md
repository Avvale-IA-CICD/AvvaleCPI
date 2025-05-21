**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iflow contains `callActivity` steps with generic names like "Custom Status". These names should be more descriptive.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iflow uses standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` in some steps for monitoring.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iflow sets a custom header called "Step" that´s enhanced for logging purposes.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains values for source, target, and description.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ`. It should follow Java notation (e.g., `com.example.seda.iflow`).

- **Parameter Externalization** -> 🟢 Ok\
    The iflow externalizes parameters using placeholders like `{{SEDA_MAIN_QUEUE}}`, `{{Number of Concurrent Processes}}`, `{{Maximum Retry Interval}}`, `{{Retry Interval}}`, `{{Retention Threshold 4 Alerting}}`, `{{Expiration Period}}`, and `{{MaxRetries}}`.

- **Error Handling** -> 🟢 Ok\
    The iflow implements error handling using Error Subprocesses in each process and a global exception subprocess that logs the exception.

- **Local Script Security** -> 🟢 Ok\
    The iflow does not use classes from `com.sap.it.api.securestore` or `com.sap.it.api.keystore` in its Groovy scripts.

- **Iflow Organization** -> 🟢 Ok\
    The iflow does not have more than 10 "callActivity" in the same "SequenceFlow".

- **Iflow Attachments** -> 🟢 Ok\
    The iflow does not appear to be creating attachments for successful messages using `messageLogFactory` in Groovy scripts.

- **IDoc Rules** -> 🟡 Does not apply\
    The iflow does not process IDocs.

- **File Rules** -> 🟡 Does not apply\
    The iflow does not process files directly.