**iFlowId**: EMCS_AEAT_-_REPSOL - **iFlowVersion**: 1.0.6

**Mermaid Diagram**
```mermaid
graph LR
    BC_SENDER[BC_SENDER] --> StartEvent2(StartEvent)
    StartEvent2 --> CallActivity20(Maintain Headers)
    CallActivity20 --> CallActivity95246099("Extract doc CD815A and parameters")
    CallActivity95246099 --> CallActivity95246175(CH_STEP)
    CallActivity95246175 --> CallActivity95246103("Base64 Encoder")
    CallActivity95246103 --> CallActivity80508921("Prepare body to SIA")
    CallActivity80508921 --> CallActivity95245819("Log Request")
    CallActivity95245819 --> ServiceTask7(Request Reply)
    ServiceTask7 -- ProcessDirect -- FIRMA_SIAVAL[FIRMA_SIAVAL]
    ServiceTask7 --> CallActivity95245816("Log Response")
    CallActivity95245816 --> CallActivity95246166("Create Structure to Send Documentum")
    CallActivity95246166 --> CallActivity95246172("Save To Send Documentum")
    CallActivity95246172 --> CallActivity95245924("Save Header FacturaeFirmada")
    CallActivity95245924 --> CallActivity95246105("Base64 Decoder 1")
    CallActivity95246105 --> ExclusiveGateway95246140(Router 7)
    ExclusiveGateway95246140 -- Route 1 -- CallActivity95246108(DS AEAT)
    CallActivity95246108 --> EndEvent95246157(EndEvent)
    ExclusiveGateway95246140 -- Route 2 -- CallActivity95246143("Convert Headers into Properties")
    CallActivity95246143 --> CallActivity95246144("Delete Headers")
    CallActivity95246144 --> ServiceTask95246145("Request Reply 2")
    ServiceTask95246145 --> CallActivity95246212("CH_Extract Root Tag")
    CallActivity95246212 --> CallActivity95246214("Generate Response Body")
    CallActivity95246214 --> EndEvent95246157
    CallActivity95246172 --> DS_FIRMA[DS_FIRMA]
    
    DS_AEAT[DS_AEAT] --> StartEvent95246114(StartEvent)
    StartEvent95246114 --> ExclusiveGateway95246120("Retry?")
    ExclusiveGateway95246120 -- Discard -- CallActivity95246121("Log Discarded Message")
    CallActivity95246121 --> EndEvent95246122("Escalation End 2")
    ExclusiveGateway95246120 -- Route 1 -- CallActivity95246135("Convert Headers into Properties")
    CallActivity95246135 --> CallActivity95246138("Delete Headers")
    CallActivity95246138 --> ServiceTask95246116("Request Reply 1")
    ServiceTask95246116 -- SOAP -- AEAT[AEAT]
    ServiceTask95246116 --> CallActivity95246208("CH_Extract Root Tag")
    CallActivity95246208 --> CallActivity95246218("Generate Response Body - Copy")
    CallActivity95246218 --> EndEvent95246115("End 9")
    
    CallActivity95246172 --> CallActivity95246177("Payload to JX0")
    CallActivity95246177 --> ServiceTask95246189("Request Reply 3")
    ServiceTask95246189 -- SOAP -- DOCUMENTUM[DOCUMENTUM]
    ServiceTask95246189 --> CallActivity95245822("Log Response")
    CallActivity95246198 --> CallActivity80508927("Set Custom Headers")
    CallActivity80508927 --> CallActivity95246180("Base64 Encoder 1")
    
    
    CallActivity8287205("Log Exception") --> ExclusiveGateway8287208("Notification?")
    StartEvent80508843("Start 4") --> CallActivity8287205("Log Exception")
    ExclusiveGateway8287208 -- true -- ServiceTask80508853("Notification")
    ServiceTask80508853 -- ProcessDirect -- AlertReceiver[AlertReceiver]
    ServiceTask80508853 --> EndEvent80508844("End 4")
    ExclusiveGateway8287208 -- false --> EndEvent80508844("End 4")
```
**BPMN Diagram**

![BPMN Diagram](./EMCS_AEAT_-_REPSOL-1.0.6.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow handles the electronic submission of documents to the Spanish Tax Agency (AEAT) and archiving in Documentum. It involves signing documents, transforming messages, and managing exceptions. It retrieves data from DataStores, interacts with SOAP services, and integrates with Documentum for archiving.

- **Involved systems with Adapters Type and Endpoint Type**
    - BC_SENDER - SOAP - EndpointSender
    - FIRMA_SIAVAL - ProcessDirect - EndpointRecevier
    - AEAT - SOAP - EndpointRecevier
    - AEAT_Actual - SOAP - EndpointRecevier
    - DOCUMENTUM - SOAP - EndpointRecevier
    - DS_AEAT - DataStoreConsumer - EndpointSender
    - DS_FIRMA - DataStoreConsumer - EndpointSender
    - AlertReceiver - ProcessDirect - EndpointRecevier

- **Key steps**
    1. Receive document from BC_SENDER via SOAP adapter.
    2. Extract payload and parameters for signing using Groovy script.
    3. Prepare the body for signing using a Groovy script to set the Headers.
    4. Call FIRMA_SIAVAL via ProcessDirect to sign the document.
    5. Generate Response Body to encapsulate signed document.
    6. Prepare the signed document structure to send to Documentum.
    7. Save the signed document to Documentum using the SOAP adapter.
    8. Save the signed document for sending it to AEAT, using datastore adapter.
    9. Consume and send the saved document to AEAT, using DataStore and SOAP adapters.
    10. Handle exceptions and send notifications via ProcessDirect to AlertReceiver.

- **Message transformation**
    - `Extract payload to sign.groovy`: Extracts relevant parts of the payload and parameters for signing.
    - `CH_STEP_FIRMA y Envio AEAT.groovy`: Set of Headers required for signing and submission to AEAT.
    - `Extraer RootName.groovy`: Extracts the root tag name from the message.
    - `Conversion headers a properties.groovy`: Converts headers into properties.
    - `Prepare body to SIA`: Prepares the body for signing by FIRMA_SIAVAL.
    - `Create Structure to Send Documentum`: Creates the XML structure for archiving in Documentum.
    - `Generate Response Body`: Encloses the signed document within a response structure.
    - `Payload to JX0`: Transform payload to Documentum required structure to use SOAP adapter.

- **Externalized parameters list, configured values (read from parameters.prop) and their descriptions**
    - `data_firma`: ZFACTURAE_FRM_FIRMADO - Name of the DataStore for signed invoice.
    - `PD_Documentum`: /modules/documentManager/documentum/documents/archiveSAP - Process Direct address for Documentum archiving.
    - `PathDocumentum`: /D.E.Marketing Europa/Facturas/Sin Procesar - Documentum path.
    - `SENDER_AUTH`: RoleBased - Sender authentication type.
    - `SENDER_BC`: Sender - Sender system name.
    - `LocationID`: SCC_INT_SUITE_AWS_EU - Location ID for auditing/logging.
    - `TimeoutUK2`: 120000 - Timeout setting.
    - `DS_NAME`: ZFACTURAE_FRM - DataStore name.
    - `UserDocumentum`: SVC_TSAPFACGLP@rg.repsol.com - Documentum user.
    - `HostUX2`: http\://portaluk2.rg.repsol.com\:2543/sap/bc/srt/Idoc - Host for UX2 system.
    - `RepositorioDocumentum`: reptestdocum - Documentum repository name.
    - `DS_FTP`: DS_FTP - DataStore name for FTP.
    - `Sender_Endpoint`: /AEAT/EMCS - Sender endpoint.
    - `FacType`: do_fac_glfdeac - Factura type.
    - `DS_MAIL_ZFACTURAE_FRM`: DS_MAIL_ZFACTURAE_FRM - DataStore for Mail
    - `BAPIRET`: BAPIRET2 - BAPI Return
    - `PrivateKeyLoginAeat`: \${property.NIF} - Private key login for AEAT.
    - `SENDER_ENDPOINT`: /ZFACTURAE - Sender endpoint.
    - `ELK_AUTH`: ELK_LOGGER - ELK authentication.
    - `Logging`: true - Enable logging.
    - `ELK_LOCATION_ID`:  - ELK location ID.
    - `AEAT_ADDRESS`: https\://prewww1.aeat.es/wlpl/inwinvoc/es.aeat.dit.adu.adi1.emcssw.Ie815V32SOAP - AEAT SOAP address.
    - `MAX_RETRIES`: 2 - Maximum retries for DataStore operations.
    - `DS_Bapiret2`: DS_Bapiret2 - Datastore for BAPIRET2.
    - `DS_AEAT`: DS_AEAT - DataStore name for AEAT.
    - `Credential_UX2`: SAP UK2 - Credential name for UX2.
    - `ELK_ENDPOINT`: https\://ingestaelastic.repsol.com\:9200/logs_isuite_poc/_doc - ELK endpoint.
    - `SMTP`: smtp.repsol.com\:25 - SMTP server.
    - `Email_Notification`: true - Enable email notification.
    - `SAP_MessageType`: CD815A - SAP Message Type.
    - `AuthJX0`: AuthJX0 - Authentication for DocumentumJX0
    - `ReqSignedToDocumentum`: ReqSignedToDocumentum - DataStore name
    - `DS_Mail_Notif`: DS_Mail_Notif - Datastore name
    - `DocumentumJX0`: http\://portaljk0.rg.repsol.com\:443/ActualizacionBandejaService/EMCSInternoActualizacionBandeja - Documentum SOAP endpoint.
    - `TimeoutMail`: 30000 - Timeout for email.
    - `ELK_PROXY_TYPE`: Internet - ELK Proxy Type

- **DataStore / JMS Dependency**
  Yes

- **Cloud Connector Dependency**
  Yes

- **Common Scripts Dependency**
    - Common_-_Groovy_Logging_Scripts (scriptBundleId)
        - Log_XML_Request.groovy
        - Log_XML_Response.groovy
        - Log_Discarded_Message.groovy
        - Log_Exception.groovy

- **ProcessDirect ComponentType Dependency**
    - /modules/Signature/SignDoc
    - /common/snowIncident