**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD]
    ERP[ERP]
    StartEvent[Start]
    Mapping[Mapping]
    EndEvent[End]

    COD -- SOAP Adapter --> StartEvent
    StartEvent --> Mapping
    Mapping --> EndEvent
    EndEvent -- SOAP Adapter --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an End-to-End connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
  - COD (EndpointSender): SOAP Adapter
  - ERP (EndpointRecevier): SOAP Adapter

- **Key steps**
 1. The iFlow starts upon receiving a message from COD via SOAP.
 2. The message is transformed using a mapping `COD_ERP_CheckEnd2EndConnectivity.opmap`.
 3. The transformed message is sent to ERP via SOAP.

- **Message transformation**
  - `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping is used.

- **Externalized parameters list, configured values and their descriptions**
  - `ERP_authentication_5`: Client Certificate
  - `Protocol-Hostname-Port`: https\://erphost\:443
  - `subject`: cn\=subject
  - `artifactname`: (Empty value, likely a placeholder for credential name)
  - `p-key-alias`: (Empty value, likely a placeholder for private key alias)
  - `ERP_allowChunking_3`: 1
  - `issuer`: cn\=issuer
  - `ERP_proxyType_4`: default
  - `COD_enableBasicAuthentication_3`: true
  - `COD_wsdlURL_1`: /wsdl/CheckConnectivityConsumer.wsdl
  - `ERP_cleanupHeaders_2`: 1
  - `location-id`: (Empty value, likely a placeholder for location ID)
  - `Client`: 100
  - `COD_address_2`: /COD/ERP/SimpleConnect

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found