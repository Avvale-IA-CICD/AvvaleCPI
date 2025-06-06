markdown
**iFlowId**: MMZ_-_IDOC_-_Single_Receiver_with_ACK - **iFlowVersion**: 1.0.0

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    The iFlow contains "callActivity" steps with default names like "Set Custom Status". These should be renamed to reflect their specific function within the iFlow for better clarity.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow uses standard headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` which aid in monitoring.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow uses custom headers "IDocOut", "IDocIn", and "SNDPRN" in the scripts "csIdocOut" and "csIdocIn" for enhanced payload search and filtering.

- **Iflow Metadata** -> 🟢 Ok\
    The `metainfo.prop` file contains the `description`, `source`, and `target` metadata.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` is `MMZ_-_IDOC_-_Single_Receiver_with_ACK`. It should follow Java notation (e.g., `com.example.idoc.receiver`).

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow externalizes parameters such as URLs, credentials, location IDs, and retry intervals using properties files (e.g., `ReceiverAddress`, `SS4_CREDENTIAL`, `LOCATION_ID`).

- **Error Handling** -> 🟢 Ok\
    The iFlow includes error handling logic within sub-processes (`Process Delivery Exception`, `Process Logic Exception`) using error start events and script tasks to parse errors and set custom statuses.

- **Local Script Security** -> 🟢 Ok\
    The Groovy scripts do not use the `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages.

- **Iflow Organization** -> 🟢 Ok\
    The iFlow does not appear to have any sequence flows with more than 10 "callActivity" steps.

- **Iflow Attachments** -> 🔴 Check Required\
    The groovy scripts "csIdocOut" and "csIdocIn" create MessageLog attachments, making use of class messageLogFactory during groovy scripting. This might represent a security/resource consumption issue, only failed messages should be logged, normally in exception process.

- **IDoc Rules** -> 🟢 Ok\
    The iFlow logs the `IDOCNUM` value as a custom header. The "Init Message" call activity extracts `DOCNUM` header which is the IDoc number, also scripts "csIdocOut" and "csIdocIn" log the IDoc number to custom headers.

- **File Rules** -> 🟡 Does not apply\
    This rule is not applicable as the iFlow does not directly deal with files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow uses an IDoc sender adapter. The property `senderAuthType` is `RoleBased` and `userRole` is externalized. However, it's recommended not to use ESBMessaging.send role, ensure the iflow not is configured to allow Basic Auth and confirm whether the assigned custom role has excessive permissions.
