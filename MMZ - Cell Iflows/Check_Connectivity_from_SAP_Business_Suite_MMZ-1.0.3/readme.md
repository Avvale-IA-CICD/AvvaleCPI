**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP System] --> StartEvent
    StartEvent[Start] --> Mapping
    Mapping[Mapping: ERP_COD_ConnectivityCheck] --> EndEvent
    EndEvent[End] --> COD[COD System]
    style ERP fill:#f9f,stroke:#333,stroke-width:2px
    style COD fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from an SAP ERP system to SAP Cloud for Customer (C4C) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP: SOAP Adapter (Sender)
    - COD: SOAP Adapter (Receiver)

- **Key steps**
    1. Receive message from ERP system via SOAP adapter.
    2. Execute message mapping (ERP_COD_ConnectivityCheck).
    3. Send message to COD system via SOAP adapter.

- **Message transformation**
    - Operation Mapping: ERP_COD_ConnectivityCheck.opmap transforms the message from ERP format to C4C format.

- **Externalized parameters list and their descriptions**
    - ERP_enableBasicAuthentication_8: Enables basic authentication for the ERP endpoint.
    - subject: Subject for ERP endpoint.
    - issuer: Issuer for ERP endpoint.
    - ERP_address_1: Address of the ERP SOAP endpoint.
    - ERP_wsdlURL_0: WSDL URL for the ERP SOAP endpoint.
    - Host: Hostname for the COD endpoint URL.
    - Port: Port for the COD endpoint URL.
    - COD_enableBasicAuthentication_6: Enables basic authentication for the COD endpoint.
    - artifactname: Credential name for the COD endpoint.
    - pr-key-alias: Private key alias for COD endpoint.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found