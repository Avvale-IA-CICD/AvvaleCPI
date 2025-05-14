**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    A[COD] --> B(StartEvent)
    B --> C{Mapping}
    C --> D(EndEvent)
    D --> E[ERP]
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD: SOAP Adapter, EndpointSender
    - ERP: SOAP Adapter, EndpointRecevier

- **Key steps**
    1.  The iFlow starts with a message from COD via SOAP.
    2.  A mapping step `COD_ERP_CheckEnd2EndConnectivity` transforms the message.
    3.  The transformed message is sent to ERP via SOAP.

- **Message transformation**
    - The iFlow uses the `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping to transform the message between COD and ERP.

- **Externalized parameters list and their descriptions**
    - COD_enableBasicAuthentication_3: Enables basic authentication for COD.
    - subject: Subject for COD.
    - issuer: Issuer for COD.
    - COD_address_2: Address for COD endpoint.
    - COD_wsdlURL_1: WSDL URL for COD endpoint.
    - Protocol-Hostname-Port: Protocol, hostname, and port for ERP endpoint.
    - Client: Client for ERP endpoint.
    - ERP_proxyType_4: Proxy type for ERP endpoint.
    - location-id: Location ID for ERP endpoint.
    - ERP_authentication_5: Authentication method for ERP endpoint.
    - artifactname: Credential name for ERP endpoint authentication.
    - ERP_allowChunking_3: Allows chunking for ERP endpoint.
    - ERP_cleanupHeaders_2: Cleans up headers for ERP endpoint.
    - p-key-alias: Private Key Alias

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found