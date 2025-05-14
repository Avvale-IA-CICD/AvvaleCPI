**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    A[COD] --> B(Start Event)
    B --> C{Mapping}
    C --> D(End Event)
    D --> E[ERP]
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#ccf,stroke:#333,stroke-width:2px
```
**Functional Summary**
**Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

**Involved systems with Adapters Type and Endpoint Type**
- COD: SOAP Adapter, EndpointSender
- ERP: SOAP Adapter, EndpointRecevier

**Key steps**
1.  Receive a request from COD via SOAP.
2.  Map the request message using a mapping step.
3.  Send the mapped message to ERP via SOAP.

**Message transformation**
- The iFlow uses a mapping step named "COD_ERP_CheckEnd2EndConnectivity" to transform the message from COD to ERP.

**Externalized parameters list and their descriptions**
- COD_enableBasicAuthentication_3:  Enables basic authentication for COD.
- subject: Subject for COD.
- issuer: Issuer for COD.
- COD_address_2: Address for the COD endpoint.
- COD_wsdlURL_1: WSDL URL for the COD endpoint.
- Protocol-Hostname-Port: Protocol, Hostname, and Port for the ERP endpoint.
- Client: Client ID for the ERP system.
- ERP_proxyType_4: Proxy type for ERP connection.
- location-id: Location ID for ERP endpoint.
- ERP_authentication_5: Authentication method for the ERP endpoint.
- artifactname: Credential name for ERP authentication.
- ERP_allowChunking_3: Allows chunking for ERP connection.
- ERP_cleanupHeaders_2: Cleans up headers for ERP connection.
- p-key-alias: Private Key Alias.

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found