**iFlowId**: avv.pij2ws.formacion.consultapais - **iFlowVersion**: 1.0.2

**Mermaid Diagram**
```mermaid
graph LR
    SFTP_SERVER[SFTP_SERVER] --> SFTP_StartEvent(StartEvent) : SFTP
    SFTP_StartEvent --> MaintainHeaders(MaintainHeaders)
    MaintainHeaders --> LogSFTPFilename(Log SFTP Filename)
    LogSFTPFilename --> RequestCapital(Request capital)
    RequestCapital --> EndEvent(EndEvent)
    EndEvent --> PIJ[PIJ] : SFTP
    MaintainHeaders --> ExceptionSubprocess(Exception Subprocess)
    subgraph ExceptionSubprocess
        ErrorStartEvent(Error Start) --> HandleException(Handle Exception)
        HandleException --> LogDiscardedMessage(Log Discarded Message)
        LogDiscardedMessage --> SetArchiveData(SetArchiveData)
        SetArchiveData --> EscalationEndEvent(Escalation End)
    end
    subgraph HandleException
        StartEvent2(Start 2) --> LogException(Log Exception)
        LogException --> NotificationGateway(Notification?)
        NotificationGateway -- true --> SendMail(Send Mail)
        NotificationGateway -- false --> EndEvent2(End 2)
        SendMail --> EndEvent2
        NotificationGateway --> ExceptionSubprocess2(Exception Subprocess 2)
        subgraph ExceptionSubprocess2
            ErrorStartEvent2(Error Start 2) --> LogException2(Log Exception)
            LogException2 --> EscalationEndEvent2(Escalation End)
        end
    end
    SendMail --> AlertReceiver[AlertReceiver] : ProcessDirect
```
**BPMN Diagram**

![BPMN Diagram](./avv.pij2ws.formacion.consultapais-1.0.2.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow retrieves XML files from an SFTP server, extracts the country name, calls a SOAP service to get the capital city, and then writes the response to another SFTP server. Error handling is included with email notification.

- **Involved systems with Adapters Type and Endpoint Type**
    - SFTP_SERVER: SFTP Adapter (Sender), EndpointSender
    - RECEIVER: SOAP Adapter (Receiver), EndpointRecevier
    - PIJ: SFTP Adapter (Receiver), EndpointRecevier
    - AlertReceiver: ProcessDirect Adapter (Receiver), EndpointRecevier

- **Key steps**
    1.  Receive XML file from SFTP server.
    2.  Log the SFTP filename.
    3.  Call SOAP service to request capital city.
    4.  Write the SOAP response to an SFTP server.
    5.  Handle any exceptions that occur during the process (logging and potential email notification).

- **Message transformation**
    - The iFlow receives an XML file, extracts data, and uses it to call a SOAP service.
    - The response from the SOAP service is then written to a file.
    - The "Maintain Headers" step creates and populates headers and properties like ArchiveName, ArchiveDirectories, and others.

- **Externalized parameters list, configured values and their descriptions**
    - Logging: true (Enables logging)
    - SFTP_SERVER: sappisftp:22 (SFTP server address)
    - EMAIL_ALERT_SENDER:  (Email address for alert sender, but value not defined)
    - ArchiveErrorsDir: (Directory for archiving error files, but value not defined)
    - SFTP_ARCHIVE_DIR: (SFTP archive directory, but value not defined)
    - SENDER_BC: PIJ (Sender Business Component)
    - SFTP_MASK: Request*.xml (File mask for SFTP)
    - RECEIVER_BC: WS_Country (Receiver Business Component)
    - Email_Notification: true (Enables email notifications for errors)
    - ArchiveSuccessfulDir: (Directory for archiving successful files, but value not defined)
    - EMAIL_SERVER_CREDS: (Credentials for the email server, but value not defined)
    - SAP_MessageType: (SAP Message Type, but value not defined)
    - EMAIL_SERVER: (Email server address, but value not defined)
    - SFTP_CREDENTIALS: PIJ_SFTP_CREDENTIALS (Credentials for SFTP access)
    - SCHEDULER: Cronjob - Every 10 seconds.
    - SFTP_DIR: ../pijadm/usr/sap/PIJ/SCPI_TESTS/MAR/FormacionSCPI (Directory on the SFTP server to read from.)

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
    - Common_-_Groovy_Logging_Scripts: Log_Filename.groovy
    - Common_-_Groovy_Logging_Scripts: Log_Discarded_Message.groovy
    - Common_-_Groovy_Logging_Scripts: Log_Exception.groovy

- **ProcessDirect ComponentType Dependency**
    - /common/errorNotification