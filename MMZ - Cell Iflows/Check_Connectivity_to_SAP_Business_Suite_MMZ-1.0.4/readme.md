**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
- **Visual representation of the flow**

```mermaid
graph LR
    COD[COD] --> StartEvent
    StartEvent[Start Event] --> Mapping
    Mapping[Mapping] --> EndEvent
    EndEvent[End Event] --> ERP[ERP]
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (C4C) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD (SAP Cloud for Customer): SOAP Adapter, EndpointSender
    - ERP (SAP ERP): SOAP Adapter, EndpointRecevier

- **Key steps**
    1.  Receive a request from COD via SOAP adapter.
    2.  Execute a mapping to transform the message.
    3.  Send the transformed message to ERP via SOAP adapter.

- **Message transformation**
    - A mapping step (`COD_ERP_CheckEnd2EndConnectivity.opmap`) transforms the message between the COD and ERP systems.

- **Externalized parameters list and their descriptions**
    - `COD_enableBasicAuthentication_3`: Enables basic authentication for the COD sender adapter.
    - `subject`: Subject for the COD sender adapter.
    - `issuer`: Issuer for the COD sender adapter.
    - `COD_address_2`: Address for the COD sender adapter.
    - `COD_wsdlURL_1`: WSDL URL for the COD sender adapter.
    - `Protocol-Hostname-Port`: Protocol, hostname, and port for the ERP receiver adapter.
    - `Client`: Client for the ERP receiver adapter.
    - `ERP_proxyType_4`: Proxy type for the ERP receiver adapter.
    - `location-id`: Location ID for the ERP receiver adapter.
    - `ERP_authentication_5`: Authentication type for the ERP receiver adapter.
    - `artifactname`: Credential name for the ERP receiver adapter.
    - `ERP_allowChunking_3`: Allows chunking for the ERP receiver adapter.
    - `ERP_cleanupHeaders_2`: Cleans up headers for the ERP receiver adapter.
    - `p-key-alias`: Private key alias for the ERP receiver adapter.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found