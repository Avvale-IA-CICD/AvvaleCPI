**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    Some call activities have standard names like "Set Headers", "Custom Status". This should be reviewed.
- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers such as SAP_Sender, SAP_Receiver, and SAP_MessageType.
- **Monitoring Custom Headers** -> 🔴 Check Required\
    The iFlow uses Custom Headers with hardcoded values, but it could be enhanced including dynamic values from Payload.
- **Iflow Metadata** -> 🟢 Ok\
    The file metainfo.prop contains values for source, target, and description.
- **Iflow Id** -> 🔴 Check Required\
    The Bundle-SymbolicName in MANIFEST.MF (SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ) uses underscores. It should follow Java notation (e.g., com.example.iflow).
- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes parameters, such as Data Store Name, Poll Interval, Retry Interval, Lock Timeout, Maximum Retry Interval, Exponential Backoff and RoleName.
- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling using error sub-processes and logging exceptions.
- **Local Script Security** -> 🔴 Check Required\
    The iFlow uses Groovy scripts. A review is required for classes from packages com.sap.it.api.securestore or com.sap.it.api.keystore.
- **Iflow Organization** -> 🟢 Ok\
    The iFlow does not have more than 10 "callActivity" in the same "SequenceFlow".
- **Iflow Attachments** -> 🔴 Check Required\
    A review is required to check for attachment creation using messageLogFactory in Groovy scripts.
- **IDoc Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process IDocs.
- **File Rules** -> 🟡 Does not apply\
    The iFlow does not appear to process files.
- **Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with HTTPS Sender Adapter, and a parameter for RoleName is defined, a review is required to check if the iflow not is configured to allow Basic Auth and uses ESBMessaging.send role, either hardcoded in BPMN XML or in Configured values, that´s considered insecure.