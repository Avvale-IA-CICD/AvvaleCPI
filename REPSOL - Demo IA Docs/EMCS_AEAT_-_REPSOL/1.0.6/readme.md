markdown
**iFlowId**: EMCS_AEAT_-_REPSOL - **iFlowVersion**: 1.0.6

**Mermaid Diagram**
- **Visual representation of the flow**
```mermaid
graph LR
    BC_SENDER --> SOAP[StartEvent_2]
    SOAP --> PrepareBody[CallActivity_20 Maintain Headers]
    PrepareBody --> ExtractPayload[CallActivity_95246099 Extract doc CD815A and parameters]
    ExtractPayload --> CH_STEP[CallActivity_95246175 CH_STEP]
    CH_STEP --> Base64Encode[CallActivity_95246103 Base64 Encoder]
    Base64Encode --> PrepareBodySIA[CallActivity_80508921 Prepare body to SIA]
    PrepareBodySIA --> LogRequest[CallActivity_95245819 Log Request]
    LogRequest --> SIAService[ServiceTask_7 Request Reply]
    SIAService --> LogResponse[CallActivity_95245816 Log Response]
    LogResponse --> CreateDocStructure[CallActivity_95246166 Create Structure to Send Documentum]
    CreateDocStructure --> SaveSignedDoc[CallActivity_95246172 Save To Send Documentum]
    SaveSignedDoc --> SaveHeader[CallActivity_95245924 Save Header FacturaeFirmada]
    SaveHeader --> Base64Decode[CallActivity_95246105 Base64 Decoder 1]
    Base64Decode --> Router[ExclusiveGateway_95246140 Router 7]
    Router -- Route 1 --> DSAEAT[CallActivity_95246108 DS AEAT]
    DSAEAT --> EndEvent[EndEvent_95246157 End 10]
    Router -- Route 2 --> ConvertHeaders[CallActivity_95246143 Convert Headers into Properties]
    ConvertHeaders --> DeleteHeaders[CallActivity_95246144 Delete Headers]
    DeleteHeaders --> AEATService[ServiceTask_95246145 Request Reply 2]
    AEATService --> ExtractRootName[CallActivity_95246212 CH_Extract Root Tag]
    ExtractRootName --> GenerateResponseBody[CallActivity_95246214 Generate Response Body]
    GenerateResponseBody --> EndEventAEAT[EndEvent_95246157 End 10]

    style EndEvent fill:#f9f,stroke:#333,stroke-width:2px
    style EndEventAEAT fill:#f9f,stroke:#333,stroke-width:2px
    
    subgraph Documentum
    CallActivity_95246180[Base64 Encoder 1]
    CallActivity_95246177[Payload to JX0]
    CallActivity_95245822[Log Response]
    ServiceTask_95246189[Request Reply 3]
    end
    
    style CallActivity_95246180 fill:#ccf,stroke:#333,stroke-width:2px
    style CallActivity_95246177 fill:#ccf,stroke:#333,stroke-width:2px
    style CallActivity_95245822 fill:#ccf,stroke:#333,stroke-width:2px
    style ServiceTask_95246189 fill:#ccf,stroke:#333,stroke-width:2px

    
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21 stroke:#333, stroke-width: 1.5px

    linkStyle 18 stroke:#333, stroke-width: 1.5px, color: red;

    SaveSignedDoc -- DataStore --> Start5((StartEvent_80508888))
    Process-->LogRequest2((CallActivity_95246198 Log Request))
    LogRequest2-->PayloadJX0((CallActivity_95246180 Base64 Encoder 1))
    PayloadJX0-->Request3((CallActivity_95246177 Payload to JX0))
    Request3-->SOAPService((ServiceTask_95246189 Request Reply 3))
    SOAPService -->DOCUMENTUM[Participant_80508889]
    SOAPService -->LogResponse2((CallActivity_95245822 Log Response))
    LogResponse2-->EndDocument((EndEvent_95246193 End 11))
    Start5-.->Retry((ExclusiveGateway_80508899 Retry?))

    Start5-- DataStore(JDBC) -->Retry
    Retry -->Step(CallActivity_80508900 Maintain Headers)
    Step-->Process
    Step-->LogRequest2

    linkStyle 22,23,24,25,26 stroke:#333, stroke-width: 1.5px
    
    DS_AEAT --> AEATUsingPD((StartEvent_95246114))
    AEATUsingPD-->CallHeaderProp(CallActivity_95246135)
    CallHeaderProp-->DeleteHead(CallActivity_95246138)
    DeleteHead-->Request1(ServiceTask_95246116)
    Request1-->ExtractName(CallActivity_95246208)
    ExtractName-->ResponseBody(CallActivity_95246218)
    ResponseBody-->End8(EndEvent_95246115)

    style AEATUsingPD fill:#ccf,stroke:#333,stroke-width:2px
    style CallHeaderProp fill:#ccf,stroke:#333,stroke-width:2px
    style End8 fill:#ccf,stroke:#333,stroke-width:2px
    style DeleteHead fill:#ccf,stroke:#333,stroke-width:2px
    style Request1 fill:#ccf,stroke:#333,stroke-width:2px
    style ExtractName fill:#ccf,stroke:#333,stroke-width:2px
    style ResponseBody fill:#ccf,stroke:#333,stroke-width:2px

    linkStyle 27,28,29,30,31,32 stroke:#333, stroke-width: 1.5px

    FIRMA_SIAVAL-. ProcessDirect .->AEAT_Actual
    AlertReceiver -.. ProcessDirect ..> HandleException
    linkStyle 33 stroke:#333, stroke-width: 1.5px, stroke-dasharray: 5 5
    linkStyle 34 stroke:#333, stroke-width: 1.5px, stroke-dasharray: 5 5
    style HandleException fill:#ccf,stroke:#333,stroke-width:2px

    
```
**BPMN Diagram**

![BPMN Diagram](./EMCS_AEAT_-_REPSOL-1.0.6.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow manages the communication with AEAT (Spanish Tax Agency) for EMCS (Excise Movement and Control System) related documents, involving document signing, archiving, and exception handling. It interacts with multiple systems to process and transmit data related to excise movements.

- **Involved systems with Adapters Type and Endpoint Type**
    - BC_SENDER - SOAP - EndpointSender
    - DS_FIRMA - DataStoreConsumer - EndpointSender
    - DS_AEAT - DataStoreConsumer - EndpointSender
    - FIRMA_SIAVAL - ProcessDirect - EndpointRecevier
    - AlertReceiver - ProcessDirect - EndpointRecevier
    - DOCUMENTUM - SOAP - EndpointRecevier
    - AEAT - SOAP - EndpointRecevier
    - AEAT_Actual - SOAP - EndpointRecevier

- **Key steps**
    1. Receive SOAP message from BC_SENDER.
    2. Extract document from payload and prepare to sign.
    3. Send document for signing via ProcessDirect to FIRMA_SIAVAL.
    4. Convert signed document to base64 and create a structure to send to Documentum.
    5. Save the base64 document to Datastore "ReqSignedToDocumentum".
    6. Send the AEAT Document to AEAT via SOAP Adapter
    7. Process response from AEAT, extracts root name from xml.
    8. Log requests and responses.
    9. Handle exceptions and send notifications.
    10. Save the request to Datastore "DS_AEAT".
    11. Archive document to Documentum via SOAP

- **Message transformation**
    - Extract payload for signing using a Groovy script ("Extract payload to sign.groovy").
    - Prepare the body for signing service SIA using an Enricher.
    - Create the XML structure for sending to Documentum using an Enricher.
    - Convert the response to the format required for AEAT.
    - Encode / Decode Base64 for signature purposes and Documentum.

- **Externalized parameters list, configured values (read from parameters.prop) and their descriptions**
    - data_firma: ZFACTURAE_FRM_FIRMADO (Name of data firma)
    - PD_Documentum: /modules/documentManager/documentum/documents/archiveSAP (ProcessDirect endpoint for Documentum archiving)
    - PathDocumentum: /D.E.Marketing Europa/Facturas/Sin Procesar (Path to archive in Documentum)
    - SENDER_AUTH: RoleBased (Sender authentication type)
    - SENDER_BC: Sender (Sender system)
    - LocationID: SCC_INT_SUITE_AWS_EU (Location ID)
    - TimeoutUK2: 120000 (Timeout for UK2)
    - DS_NAME: ZFACTURAE_FRM (Name of Datastore for firma)
    - UserDocumentum: SVC_TSAPFACGLP@rg.repsol.com (Documentum user)
    - HostUX2: http://portaluk2.rg.repsol.com:2543/sap/bc/srt/Idoc (Host for UX2)
    - RepositorioDocumentum: reptestdocum (Documentum repository)
    - DS_FTP: DS_FTP (Datastore for FTP)
    - Sender_Endpoint: /AEAT/EMCS (Sender endpoint)
    - FacType: do_fac_glfdeac (Type of factura)
    - DS_MAIL_ZFACTURAE_FRM: DS_MAIL_ZFACTURAE_FRM (Data store Mail)
    - BAPIRET: BAPIRET2 (BAPI Result)
    - PrivateKeyLoginAeat: \${property.NIF} (Private key alias for AEAT login)
    - SENDER_ENDPOINT: /ZFACTURAE (Sender Endpoint)
    - ELK_AUTH: ELK_LOGGER (Authentication for ELK)
    - Logging: true (Enable/disable logging)
    - ELK_LOCATION_ID: (ELK Location ID)
    - AEAT_ADDRESS: https://prewww1.aeat.es/wlpl/inwinvoc/es.aeat.dit.adu.adi1.emcssw.Ie815V32SOAP (AEAT endpoint address)
    - MAX_RETRIES: 2 (Maximum retries for DataStore consumer)
    - DS_Bapiret2: DS_Bapiret2 (Datastore for BAPIRET2)
    - DS_AEAT: DS_AEAT (Datastore for AEAT requests)
    - Credential_UX2: SAP UK2 (Credential for UX2)
    - ELK_ENDPOINT: https://ingestaelastic.repsol.com:9200/logs_isuite_poc/_doc (ELK endpoint for logging)
    - SMTP: smtp.repsol.com:25 (SMTP server for email notifications)
    - Email_Notification: true (Enable/disable email notifications)
    - SAP_MessageType: CD815A (SAP Message Type)
    - AuthJX0: AuthJX0 (Authentication for JX0)
    - ReqSignedToDocumentum: ReqSignedToDocumentum (Datastore for Signed Documentum)
    - DS_Mail_Notif: DS_Mail_Notif (Data store Mail Notification)
    - DocumentumJX0: http://portaljk0.rg.repsol.com:443/ActualizacionBandejaService/EMCSInternoActualizacionBandeja (Documentum JX0 address)
    - TimeoutMail: 30000 (Timeout for Mail)
    - ELK_PROXY_TYPE: Internet (ELK Proxy Type)

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
    - Common_-_Groovy_Logging_Scripts
        - Log_XML_Request.groovy
        - Log_XML_Response.groovy
        - Log_Discarded_Message.groovy
        - Log_Exception.groovy

- **ProcessDirect ComponentType Dependency**
    - /modules/Signature/SignDoc
    - /common/snowIncident