markdown
**iFlowId**: MMZ_-_IDOC_-_Single_Receiver_with_ACK - **iFlowVersion**: 1.0.0

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required\
    Some call activities like 'DemoStep' remain with standard names which is not descriptive of the operation it performs.

- **Monitoring Standard Headers** -> 🟢 Ok\
    The iFlow is using standard headers for monitoring such as SAP_Sender, SAP_Receiver and SAP_MessageType.

- **Monitoring Custom Headers** -> 🟢 Ok\
    The iFlow is using custom headers for monitoring. The script `csIdocIn` adds custom header properties IDocIn and SNDPRN and script `csIdocOut` adds IDocOut.

- **Iflow Metadata** -> 🟢 Ok\
    The file `metainfo.prop` contains values for source, target, and description.

- **Iflow Id** -> 🔴 Check Required\
    The Bundle-SymbolicName `MMZ_-_IDOC_-_Single_Receiver_with_ACK` in `MANIFEST.MF` uses underscores and hyphens. It should use java notation with dots e.g. `com.example.idoc.receiver`.

- **Parameter Externalization** -> 🟢 Ok\
    The iFlow is externalizing important parameters like URLs, authentication data (credential name `SS4_CREDENTIAL`), LocationId, and retry intervals.

- **Error Handling** -> 🟢 Ok\
    The iFlow implements error handling with Error Subprocesses ("Process Delivery Exception" and "Process Logic Exception") that include error start events, parsing of errors (using groovy scripts), setting custom statuses, and saving error data.

- **Local Script Security** -> 🟢 Ok\
    The groovy scripts (`script1.groovy`) does not make use of clases from these packages com.sap.it.api.securestore or com.sap.it.api.keystore.

- **Iflow Organization** -> 🟢 Ok\
    There are no SequenceFlows with more than 10 callActivity elements.

- **Iflow Attachments** -> 🔴 Check Required\
    The groovy script `csIdocOut` creates a messagelog and uses `messageLogFactory`. Only failed messages should be logged, normally in exception process. Also it's enabled the property "enableMPLAttachments" for HTTP Sender Adapter.

- **IDoc Rules** -> 🟢 Ok\
    The developer is logging the IDOCNUM value as Custom Headers for input and output Idocs.

- **File Rules** -> 🟡 Does not apply\
    This iFlow does not deal with files.

- **Inbound Endpoint Rules** -> 🔴 Check Required\
    The iFlow exposes an endpoint with the IDoc Sender Adapter (`Participant_1`) that is configured with authentication `RoleBased`. It is configured to use the role `{{SENDER_ROLE}}` for authentication. Also, the adapter `MessageFlow_35` is exposing an endpoint with an IDOC Receiver Adapter with Basic Authentication.
