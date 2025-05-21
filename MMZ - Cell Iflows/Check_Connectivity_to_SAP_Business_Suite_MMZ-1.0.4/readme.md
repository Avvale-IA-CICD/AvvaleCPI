**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD] --> SOAP(StartEvent) : SOAP
    SOAP --> Mapping[Mapping]
    Mapping --> SOAP_ERP(EndEvent)
    SOAP_ERP --> ERP[ERP] : SOAP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_MMZ-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD: SOAP, EndpointSender
    - ERP: SOAP, EndpointRecevier

- **Key steps**
    1. The iFlow starts upon receiving a message from COD.
    2. The message is passed to a Mapping step.
    3. The Mapping activity (COD_ERP_CheckEnd2EndConnectivity.opmap) transforms the message.
    4. The iFlow sends the transformed message to ERP.

- **Message transformation**
    - Mapping: COD_ERP_CheckEnd2EndConnectivity.opmap

- **Externalized parameters list, configured values and their descriptions**
    - COD_address_2: /COD/ERP/SimpleConnect
    - COD_wsdlURL_1: /wsdl/CheckConnectivityConsumer.wsdl
    - COD_enableBasicAuthentication_3: true
    - ERP_proxyType_4: default
    - ERP_allowChunking_3: 1
    - ERP_cleanupHeaders_2: 1
    - ERP_authentication_5: Client Certificate
    - Protocol-Hostname-Port: https\://erphost\:443
    - Client: 100
    - location-id:
    - artifactname:
    - p-key-alias:
    - subject: cn\=subject
    - issuer: cn\=issuer

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found