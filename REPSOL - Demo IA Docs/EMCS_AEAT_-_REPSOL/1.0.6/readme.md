markdown
**iFlowId**: EMCS_AEAT_-_REPSOL - **iFlowVersion**: 1.0.6

**Mermaid Diagram**
- **Visual representation of the flow**

```mermaid
graph LR
    BC_SENDER --> StartEvent_2(Start) : SOAP
    StartEvent_2 --> CallActivity_20(Maintain Headers)
    CallActivity_20 --> CallActivity_95246099("Extract doc CD815A and parameters")
    CallActivity_95246099 --> CallActivity_95246175(CH_STEP)
    CallActivity_95246175 --> CallActivity_95246103("Base64 Encoder")
    CallActivity_95246103 --> CallActivity_80508921("Prepare body to SIA")
    CallActivity_80508921 --> CallActivity_95245819("Log Request")
    CallActivity_95245819 --> ServiceTask_7("Request Reply")
    ServiceTask_7 --> CallActivity_95245816("Log Response")
    CallActivity_95245816 --> CallActivity_95246166("Create Structure to Send Documentum")
    CallActivity_95246166 --> CallActivity_95246172("Save To Send Documentum")
    CallActivity_95246172 --> CallActivity_95245924("Save Header FacturaeFirmada")
    CallActivity_95245924 --> CallActivity_95246105("Base64 Decoder 1")
    CallActivity_95246105 --> ExclusiveGateway_95246140("Router 7")
    ExclusiveGateway_95246140 -- Route 1 --> CallActivity_95246108("DS AEAT")
    CallActivity_95246108 --> EndEvent_95246157("End 10")
    ExclusiveGateway_95246140 -- Route 2 --> CallActivity_95246143("Convert Headers into Properties")
    CallActivity_95246143 --> CallActivity_95246144("Delete Headers")
    CallActivity_95246144 --> ServiceTask_95246145("Request Reply 2")
    ServiceTask_95246145 --> CallActivity_95246212("CH_Extract Root Tag")
    CallActivity_95246212 --> CallActivity_95246214("Generate Response Body")
    CallActivity_95246214 --> EndEvent_95246157
    ServiceTask_7 -- ProcessDirect --> FIRMA_SIAVAL
    ServiceTask_95246116 -- SOAP --> AEAT
    ServiceTask_95246189 -- SOAP --> DOCUMENTUM
    Participant_8287215 -- ProcessDirect --> AlertReceiver
    Participant_95246153 -- SOAP --> AEAT_Actual
    Participant_80508887 -- DataStore --> StartEvent_80508888
    Participant_95246112 -- DataStore --> StartEvent_95246114

```
**BPMN Diagram**

![BPMN Diagram](./EMCS_AEAT_-_REPSOL-1.0.6.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
The iFlow handles the process of sending documents to AEAT (Spanish Tax Agency) and archiving them in Documentum. It retrieves requests, signs them, sends them to AEAT, and then archives the signed documents in Documentum. Error handling and logging are included.

- **Involved systems with Adapters Type and Endpoint Type**
    - BC_SENDER (SOAP, EndpointSender)
    - DS_FIRMA (DataStoreConsumer, EndpointSender)
    - DS_AEAT (DataStoreConsumer, EndpointSender)
    - FIRMA_SIAVAL (ProcessDirect, EndpointRecevier)
    - AlertReceiver (ProcessDirect, EndpointRecevier)
    - DOCUMENTUM (SOAP, EndpointRecevier)
    - AEAT (SOAP, EndpointRecevier)
    - AEAT_Actual (SOAP, EndpointRecevier)

- **Key steps**
1. Receives a request.
2. Extracts document content and signs it using Firma/SIAVAL service.
3. Sends the signed document to AEAT.
4. Creates document structure to be archived in Documentum.
5. Archives the signed document in Documentum.
6. Implements error handling and notification.
7. Logs requests, responses, and exceptions.
8. Send AEAT using PD.

- **Message transformation**
    - Extraction of payload for signing.
    - Base64 encoding/decoding.
    - XML wrapping for Documentum archiving.
    - Conversion headers into properties.
    - Extraer RootName

- **Externalized parameters list, configured values and their descriptions**
    - data_firma: ZFACTURAE_FRM_FIRMADO (Unknown description)
    - PD_Documentum: /modules/documentManager/documentum/documents/archiveSAP (Unknown description)
    - PathDocumentum: /D.E.Marketing Europa/Facturas/Sin Procesar (Unknown description)
    - SENDER_AUTH: RoleBased (Unknown description)
    - SENDER_BC: Sender (Unknown description)
    - LocationID: SCC_INT_SUITE_AWS_EU (Unknown description)
    - TimeoutUK2: 120000 (Unknown description)
    - DS_NAME: ZFACTURAE_FRM (Unknown description)
    - UserDocumentum: SVC_TSAPFACGLP@rg.repsol.com (Unknown description)
    - HostUX2: http\://portaluk2.rg.repsol.com\:2543/sap/bc/srt/Idoc (Unknown description)
    - RepositorioDocumentum: reptestdocum (Unknown description)
    - DS_FTP: DS_FTP (Unknown description)
    - Sender_Endpoint: /AEAT/EMCS (Unknown description)
    - FacType: do_fac_glfdeac (Unknown description)
    - DS_MAIL_ZFACTURAE_FRM: DS_MAIL_ZFACTURAE_FRM (Unknown description)
    - BAPIRET: BAPIRET2 (Unknown description)
    - PrivateKeyLoginAeat: \${property.NIF} (Unknown description)
    - SENDER_ENDPOINT: /ZFACTURAE (Unknown description)
    - ELK_AUTH: ELK_LOGGER (Unknown description)
    - Logging: true (Unknown description)
    - ELK_LOCATION_ID:  (Unknown description)
    - AEAT_ADDRESS: https\://prewww1.aeat.es/wlpl/inwinvoc/es.aeat.dit.adu.adi1.emcssw.Ie815V32SOAP (Unknown description)
    - MAX_RETRIES: 2 (Unknown description)
    - DS_Bapiret2: DS_Bapiret2 (Unknown description)
    - DS_AEAT: DS_AEAT (Unknown description)
    - Credential_UX2: SAP UK2 (Unknown description)
    - ELK_ENDPOINT: https\://ingestaelastic.repsol.com\:9200/logs_isuite_poc/_doc (Unknown description)
    - SMTP: smtp.repsol.com\:25 (Unknown description)
    - Email_Notification: true (Unknown description)
    - SAP_MessageType: CD815A (Unknown description)
    - AuthJX0: AuthJX0 (Unknown description)
    - ReqSignedToDocumentum: ReqSignedToDocumentum (Unknown description)
    - DS_Mail_Notif: DS_Mail_Notif (Unknown description)
    - DocumentumJX0: http\://portaljk0.rg.repsol.com\:443/ActualizacionBandejaService/EMCSInternoActualizacionBandeja (Unknown description)
    - TimeoutMail: 30000 (Unknown description)
    - ELK_PROXY_TYPE: Internet (Unknown description)

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
    - Common_-_Groovy_Logging_Scripts - Log_XML_Request.groovy
    - Common_-_Groovy_Logging_Scripts - Log_XML_Response.groovy
    - Common_-_Groovy_Logging_Scripts - Log_Discarded_Message.groovy
    - Common_-_Groovy_Logging_Scripts - Log_Exception.groovy

- **ProcessDirect ComponentType Dependency**
/modules/Signature/SignDoc
/common/snowIncident