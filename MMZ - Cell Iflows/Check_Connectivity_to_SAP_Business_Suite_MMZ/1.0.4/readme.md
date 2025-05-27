**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD]
    ERP[ERP]
    StartEvent((StartEvent))
    Mapping[Mapping]
    EndEvent((EndEvent))

    COD -- SOAP Adapter --> StartEvent
    StartEvent --> Mapping
    Mapping --> EndEvent
    EndEvent -- SOAP Adapter --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_MMZ-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD (EndpointSender) - SOAP Adapter
    - ERP (EndpointRecevier) - SOAP Adapter

- **Key steps**
    1.  Receive request from COD via SOAP adapter.
    2.  Execute message mapping "COD_ERP_CheckEnd2EndConnectivity".
    3.  Send the transformed message to ERP via SOAP adapter.

- **Message transformation**
    - Mapping: COD_ERP_CheckEnd2EndConnectivity.opmap

- **Externalized parameters list, configured values and their descriptions**
    - COD_address_2: /COD/ERP/SimpleConnect
    - COD_enableBasicAuthentication_3: true
    - COD_wsdlURL_1: /wsdl/CheckConnectivityConsumer.wsdl
    - ERP_allowChunking_3: 1
    - ERP_authentication_5: Client Certificate
    - ERP_cleanupHeaders_2: 1
    - ERP_proxyType_4: default
    - Protocol-Hostname-Port: https\://erphost\:443
    - artifactname:
    - Client: 100
    - issuer: cn\=issuer
    - location-id:
    - p-key-alias:
    - subject: cn\=subject

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found