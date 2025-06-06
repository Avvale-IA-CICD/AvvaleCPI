markdown
**iFlowId**: MMZ_-_IDOC_-_Single_Receiver_with_ACK - **iFlowVersion**: 1.0.0

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    At least one "callActivity" step ("DemoStep") uses a generic name. The steps "Parse Error" and "Set Custom Status" are repeated in multiple sub processes.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow is using standard headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow is using custom headers such as `IDocIn` and `SNDPRN` for monitoring (in `csIdocIn` script).

- **Iflow Metadata** -> 🟢 Ok\
    iFlow metadata (source, target, description) is populated in `metainfo.prop`.

- **Iflow Id** -> 🔴 Check Required\
    The `Bundle-SymbolicName` in `MANIFEST.MF` (MMZ_-_IDOC_-_Single_Receiver_with_ACK) uses underscores and hyphens instead of the recommended java class style notation with dots.

- **Parameter Externalization** -> 🟢 Ok\
    Important parameters like URLs, credential names, and datastore names are externalized as configuration parameters.

- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling with error subprocesses (`Process Delivery Exception`, `Process Logic Exception`) and custom status settings.

- **Local Script Security** -> 🟢 Ok\
    The groovy scripts don't use the security related packages `com.sap.it.api.securestore` or `com.sap.it.api.keystore`.

- **Iflow Organization** -> 🟢 Ok\
    The iflow doesn't have more than 10 "callActivity" elements in any "SequenceFlow".

- **Iflow Attachments** -> 🟢 Ok\
    The groovy scripts doesn't create attachments for succesful messages.

- **IDoc Rules** -> 🟢 Ok\
    The iFlow is logging `IDOCNUM` as a custom header (`IDocIn`) in the `csIdocIn` script.

- **File Rules** -> 🟡 Does not apply\
    This iFlow does not process files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with the `IDOC` sender adapter. The configuration `senderAuthType` is set to `RoleBased` using the `{{SENDER_ROLE}}` parameter. Verify that no Basic Authentication is enabled and that the `ESBMessaging.send` role is not being directly assigned or hardcoded. Basic Authentication is disabled as per the properties.
