markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

-

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
        - The iFlow contains several "callActivity" steps with generic names like "Step 1", "Step 2", "Custom Status", "Set Headers", "Log Async Exception". These names do not provide enough context about the specific function of each step.
    - **Monitoring Standard Headers** -> 🟢 Ok\
        - The iFlow utilizes standard headers such as `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` for monitoring purposes.
    - **Monitoring Custom Headers** -> 🟢 Ok\
        - The iFlow makes use of the `SAP_MessageProcessingLogCustomStatus` header, to set custom message status, which enhances payload search and filtering.
    - **Iflow Metadata** -> 🟢 Ok\
        - The `metainfo.prop` file contains values for `description`, `source`, and `target`.
    - **Iflow Id** -> 🔴 Check Required\
        - The `Bundle-SymbolicName` in `MANIFEST.MF` is `SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ`. It uses underscores and hyphens, which is not in Java notation. Valid notation should be like `com.sap.package.iflow`.
    - **Parameter Externalization** -> 🟢 Ok\
        - The iFlow uses externalized parameters, indicated by the `{{...}}` notation in several adapter properties and script properties.  Examples include `{{Maximum Retry Interval}}`, `{{Exponential Backoff}}`, `{{Data Store Name}}`, `{{Poll Interval}}`, `{{Retry Interval}}`, `{{Lock Timeout}}`, `{{RoleName}}`,`{{Retention Threshold 4 Alerting}}`, and `{{Expiration Period}}`.
    - **Error Handling** -> 🟢 Ok\
        - The iFlow implements error handling using Error Event Subprocesses in multiple integration processes ("Step 1 Exception Subprocess", "Step 2 Exception Subprocess", "Step 3 Exception Subprocess", "SEDA Router Exception Subprocess", "Trigger Exception Subprocess").  These subprocesses log exceptions using the "Log Async Exception" process. Additionally, the "SEDA Router" process handles discarding messages after a maximum number of retries.
    - **Local Script Security** -> 🟢 Ok\
        - No usage of classes from packages `com.sap.it.api.securestore` or `com.sap.it.api.keystore` is detected.
    - **Iflow Organization** -> 🟢 Ok\
        - The iFlow does not appear to have any SequenceFlows exceeding 10 "callActivity" steps.
    - **Iflow Attachments** -> 🟢 Ok\
        - There is no evidence of attachment creation using `messageLogFactory` in the provided Groovy scripts.
    - **IDoc Rules** -> 🟡 Does not apply\
        - The iFlow does not seem to process IDocs.
    - **File Rules** -> 🟡 Does not apply\
        - The iFlow does not directly handle files.
    - **Endpoint Rules** -> 🔴 Check Required\
        - The iFlow exposes an endpoint with `HTTPS Sender Adapter`, check if the iflow is configured to allow Basic Auth with ESBMessaging.send.
