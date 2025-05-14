**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    A[COD] --> B(Start Event)
    B --> C{Mapping}
    C --> D(End Event)
    D --> E[ERP]
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an End-to-End connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD: SOAP Adapter, Endpoint Sender
    - ERP: SOAP Adapter, Endpoint Receiver

- **Key steps**
    1. The iFlow is triggered by a message from COD via SOAP.
    2. The message is passed to a mapping step.
    3. A mapping 'COD_ERP_CheckEnd2EndConnectivity' transforms the message.
    4. The transformed message is sent to ERP via SOAP.

- **Message transformation**
    - The iFlow uses a mapping named 'COD_ERP_CheckEnd2EndConnectivity' located at 'dir://opmap/src/main/resources/mapping/COD_ERP_CheckEnd2EndConnectivity.opmap'.

- **Externalized parameters list and their descriptions**
    - COD_enableBasicAuthentication_3: Enables basic authentication for COD.
    - subject: Subject for COD.
    - issuer: Issuer for COD.
    - COD_address_2: Address for COD endpoint.
    - COD_wsdlURL_1: WSDL URL for COD endpoint.
    - Protocol-Hostname-Port: Protocol, hostname, and port of the ERP system.
    - Client: Client ID for the ERP system.
    - ERP_proxyType_4: Proxy type for ERP connection.
    - location-id: Location ID for ERP.
    - ERP_authentication_5: Authentication type for ERP connection.
    - artifactname: Credential name for ERP authentication.
    - ERP_allowChunking_3: Allows chunking for ERP connection.
    - ERP_cleanupHeaders_2: Cleans up headers for ERP connection.
    - p-key-alias: Private key alias.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found