**iFlowId:** Common_-_Error_Notification_Email - **iFlowVersion:** 1.1.5

**Best Practices Summary**
- **Iflow Steps Naming** -> 🔴 Check Required
   There are "callActivity" steps with standard names such as "log error", "Header2Attachment", "Error Mail Notification", and "Clean Headers".
- **Monitoring Standard Headers** -> 🟢 Ok
   The iflow properties define allowed headers which include `SAP_Sender` and `SAP_Receiver`.
- **Monitoring Custom Headers** -> 🟢 Ok
   The iflow properties define allowed headers which include `EMAIL_SUBJECT`, `EMAIL_RECIPIENTS`, `Attachment`, `IflowId`, and `ParentMsgId`.
- **Iflow Metadata** -> 🔴 Check Required
   The `metainfo.prop` file has an empty description.
- **Iflow Id** -> 🟢 Ok
   The `Bundle-SymbolicName` in `MANIFEST.MF` uses Java notation (dots instead of underscores or hyphens).
- **Parameter Externalization** -> 🟢 Ok
   The iflow uses externalized parameters (e.g., `{{EMAIL_SERVER}}`, `{{AUTH_TYPE}}`, `{{PROTECTION_TYPE}}`, `{{LOCATION_ID}}`, `{{EMAIL_ALERT_SENDER}}`, `{{PROXY_TYPE}}`, `{{AUTH_BASIC}}`)
- **Error Handling** -> 🟢 Ok
   The iflow includes an "Exception Subprocess" for error handling.
- **Local Script Security** -> 🟢 Ok
   The provided code doesn't show the usage of `com.sap.it.api.securestore` or `com.sap.it.api.keystore` packages.
- **Iflow Organization** -> 🟢 Ok
   No `SequenceFlow` contains more than 10 `callActivity` elements.
- **Iflow Attachments** -> 🔴 Check Required
   The iflow uses scripts ("Header2Attachment.groovy") to handle attachments, but it's not confirmed if `messageLogFactory` is used to create attachments for successful messages. The use of attachments should be checked.
- **IDoc Rules** -> 🟡 Does not apply
   The iFlow is not designed to handle IDocs.
- **File Rules** -> 🟡 Does not apply
   The iFlow is not designed to handle files.