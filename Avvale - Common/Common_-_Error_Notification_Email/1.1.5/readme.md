**iFlowId**: Common_-_Error_Notification_Email - **iFlowVersion**: 1.1.5

**Mermaid Diagram**
```mermaid
graph LR
    NotificationEmail[NotificationEmail] --> ProcessDirect((StartEvent_2))::ProcessDirect Adapter: ProcessDirect
    ProcessDirect --> Header2Attachment[Header2Attachment]
    Header2Attachment --> ErrorMailNotification[Error Mail Notification]
    ErrorMailNotification --> CleanHeaders[Clean Headers]
    CleanHeaders --> EnvioMail[Envio Mail]
    EnvioMail --> End((EndEvent_2))
    EnvioMail -- Mail Adapter --> MailAlert[MailAlert]
    SubProcess[Exception Subprocess] -- Error --> Header2Attachment
    SubProcess -- Error --> ErrorMailNotification
    SubProcess -- Error --> CleanHeaders
    SubProcess -- Error --> EnvioMail
    StartError[Error Start]:::hidden --> LogError[log error]
    LogError --> EndError[End]:::hidden
    style StartError fill:#f9f,stroke:#333,stroke-width:2px
    style EndError fill:#f9f,stroke:#333,stroke-width:2px
    style ProcessDirect fill:#f9f,stroke:#333,stroke-width:2px
    style End fill:#f9f,stroke:#333,stroke-width:2px
    style Header2Attachment fill:#ccf,stroke:#333,stroke-width:2px
    style ErrorMailNotification fill:#ccf,stroke:#333,stroke-width:2px
    style CleanHeaders fill:#ccf,stroke:#333,stroke-width:2px
    style EnvioMail fill:#f9f,stroke:#333,stroke-width:2px
    style LogError fill:#ccf,stroke:#333,stroke-width:2px
```
**BPMN Diagram**

![BPMN Diagram](./Common_-_Error_Notification_Email-1.1.5.png "BPMN Diagram")

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow is designed to send error notifications via email. It receives a message via ProcessDirect, extracts relevant information, formats an email with the error details, and sends it to the configured recipients. The iFlow also handles exception scenarios by logging the error details.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   NotificationEmail - ProcessDirect - EndpointSender
    -   MailAlert - Mail - EndpointRecevier

-   **Key steps**

    1.  Receive message via ProcessDirect.
    2.  Convert header information to attachment (Header2Attachment).
    3.  Generate Error Email Notification (Error Mail Notification).
    4.  Clean up headers.
    5.  Send email (Envio Mail).
    6.  Handle errors via Exception Subprocess.
    7.  Log Exception (log error).

-   **Message transformation**
    -   Header to Attachment using Groovy Script `HeaderToAttachment.groovy`
    -   Email Notification creation using Groovy Script `Error_Email_Notification.groovy`

-   **Externalized parameters list, configured values and their descriptions**
    -   EMAIL_ALERT_SENDER: CPI_TEST_TENANT@avvale.com - Sender email address for the alert.
    -   PROTECTION_TYPE: starttls_mandatory - Specifies the type of SSL/TLS protection for the email connection.
    -   AUTH_BASIC: smtp_cred - Credentials for basic authentication.
    -   PROXY_TYPE: none - Type of proxy used for the connection.
    -   SSL: off - Enable/disable SSL.
    -   EMAIL_SERVER: smtp-relay.brevo.com:587 - Email server address and port.
    -   AUTH: none - Authentication method used.
    -   LOCATION_ID:  - Location ID
    -   AUTH_TYPE: loginPlain - Authentication Type

-   **DataStore / JMS Dependency**
    Not Found

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    -   Common_-_Groovy_Util_Scripts
    -   Common_-_Groovy_Email_Notification_Scripts
    -   Common_-_Groovy_Logging_Scripts

-   **ProcessDirect ComponentType Dependency**
    -   /common/errorNotification