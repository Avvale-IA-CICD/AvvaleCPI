**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD] --> StartEvent(Start Event)
    StartEvent --> Mapping[Mapping]
    Mapping --> EndEvent(End Event)
    EndEvent --> ERP[ERP]
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD: SOAP Adapter, EndpointSender
    - ERP: SOAP Adapter, EndpointRecevier

- **Key steps**
    1. The iFlow starts with a message received from COD via a SOAP adapter.
    2. The message is then processed by a mapping step (COD_ERP_CheckEnd2EndConnectivity.opmap).
    3. Finally, the message is sent to ERP via a SOAP adapter.

- **Message transformation**
    - The iFlow uses a mapping named "COD_ERP_CheckEnd2EndConnectivity" located at `dir://opmap/src/main/resources/mapping/COD_ERP_CheckEnd2EndConnectivity.opmap` to transform the message.

- **Externalized parameters list and their descriptions**
    - COD_enableBasicAuthentication_3: Enables basic authentication for the COD sender adapter.
    - subject: Subject for the COD sender.
    - issuer: Issuer for the COD sender.
    - COD_address_2: Address of the COD endpoint.
    - COD_wsdlURL_1: WSDL URL for the COD endpoint.
    - Protocol-Hostname-Port: Protocol, hostname and port used for ERP endpoint address.
    - Client: Client ID for the ERP endpoint.
    - ERP_proxyType_4: Proxy type for the ERP receiver adapter.
    - location-id: Location ID for the ERP receiver adapter.
    - ERP_authentication_5: Authentication type for the ERP receiver adapter.
    - artifactname: Credential name for the ERP receiver adapter.
    - ERP_allowChunking_3: Allows chunking for the ERP receiver adapter.
    - ERP_cleanupHeaders_2: Cleans up headers for the ERP receiver adapter.
    - p-key-alias: Private key alias for ERP receiver adapter.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found