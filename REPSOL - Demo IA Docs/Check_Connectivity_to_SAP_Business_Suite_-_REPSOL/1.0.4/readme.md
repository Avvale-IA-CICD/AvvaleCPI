**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD] --> StartEvent((StartEvent)):::circle -- SOAP -->|SOAP Adapter| StartEvent
    StartEvent --> Mapping{Mapping}
    Mapping --> EndEvent((EndEvent)):::circle
    EndEvent --> ERP[ERP] -- SOAP -->|SOAP Adapter| ERP
    classDef circle fill:#f9f,stroke:#333,stroke-width:2px
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an End-to-End connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD (SAP Cloud for Customer): SOAP Adapter (EndpointSender)
    - ERP (SAP ERP): SOAP Adapter (EndpointRecevier)

- **Key steps**
    1.  Receive request from COD system.
    2.  Map the incoming message using the `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping.
    3.  Send the transformed message to the ERP system.

- **Message transformation**
    - `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping is used.

- **Externalized parameters list, configured values and their descriptions**
    - `ERP_authentication_5`: Client Certificate - Authentication method for ERP.
    - `Protocol-Hostname-Port`: `https://erphost:443` - ERP host and port.
    - `subject`: `cn=subject` - Subject.
    - `artifactname`:   - Artifact Name
    - `p-key-alias`:  - Private key alias for ERP.
    - `ERP_allowChunking_3`: 1 - Flag to allow chunking for ERP.
    - `issuer`: `cn=issuer` - Issuer.
    - `ERP_proxyType_4`: default - Proxy type for ERP connection.
    - `COD_enableBasicAuthentication_3`: true - Enable Basic Authentication for COD.
    - `COD_wsdlURL_1`: `/wsdl/CheckConnectivityConsumer.wsdl` - WSDL URL for COD.
    - `ERP_cleanupHeaders_2`: 1 - Flag to clean up headers for ERP.
    - `location-id`:   - Location ID for ERP.
    - `Client`: 100 - ERP Client.
    - `COD_address_2`: `/COD/ERP/SimpleConnect` - Address for COD.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found