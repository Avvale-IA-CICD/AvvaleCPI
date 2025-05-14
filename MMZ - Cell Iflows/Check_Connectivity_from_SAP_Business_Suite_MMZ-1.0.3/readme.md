**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] --> StartEvent((Start Event))
    StartEvent --> Mapping[Mapping: ERP_COD_ConnectivityCheck]
    Mapping --> EndEvent((End Event))
    EndEvent --> COD[COD]
    style StartEvent fill:#f9f,stroke:#333,stroke-width:2px
    style EndEvent fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (C4C) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (EndpointSender) - SOAP Adapter, HTTP Endpoint
    - COD (EndpointRecevier) - SOAP Adapter, HTTP Endpoint

- **Key steps**
    1. Receive SOAP message from ERP system.
    2. Execute message mapping "ERP_COD_ConnectivityCheck".
    3. Send SOAP message to COD system.

- **Message transformation**
    - Operation Mapping: ERP_COD_ConnectivityCheck

- **Externalized parameters list and their descriptions**
    - ERP_enableBasicAuthentication_8: Enables basic authentication for ERP endpoint.
    - subject: Subject for ERP endpoint.
    - issuer: Issuer for ERP endpoint.
    - ERP_address_1: Address of the ERP SOAP endpoint.
    - ERP_wsdlURL_0: WSDL URL of the ERP SOAP service.
    - COD_enableBasicAuthentication_6: Enables basic authentication for COD endpoint.
    - Host: Hostname of the COD system.
    - Port: Port of the COD system.
    - artifactname: Credential name for COD endpoint.
    - pr-key-alias: Private key alias for COD endpoint.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found