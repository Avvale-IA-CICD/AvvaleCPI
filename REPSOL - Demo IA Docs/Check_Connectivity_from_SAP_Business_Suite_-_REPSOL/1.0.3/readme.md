**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] --> SOAP_ERP(Start Event)
    SOAP_ERP --> Mapping
    Mapping --> SOAP_COD(End Event)
    SOAP_COD --> COD[COD]
    style ERP fill:#f9f,stroke:#333,stroke-width:2px
    style COD fill:#f9f,stroke:#333,stroke-width:2px
    SOAP_ERP -- SOAP Adapter --> Mapping
    Mapping -- SOAP Adapter --> SOAP_COD
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP, EndpointSender)
    - COD (SOAP, EndpointRecevier)

- **Key steps**
    1. Receive SOAP message from ERP system.
    2. Perform mapping using the `ERP_COD_ConnectivityCheck.opmap` operation mapping.
    3. Send SOAP message to COD system.

- **Message transformation**
    - The iFlow utilizes the `ERP_COD_ConnectivityCheck.opmap` operation mapping to transform the message between ERP and COD.

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: Configured value: `0`. Description: Enables or disables basic authentication for the COD receiver channel.
    - `subject`: Configured value: ``. Description: Subject for certificate based authentication at ERP Sender channel.
    - `ERP_wsdlURL_0`: Configured value: `/wsdl/ConnectivityCheckConsumer.wsdl`. Description: WSDL URL for the ERP sender channel.
    - `Port`: Configured value: `443`. Description: Port number for the COD receiver channel.
    - `artifactname`: Configured value: ``. Description: Credential Name for COD receiver channel authentication.
    - `ERP_enableBasicAuthentication_8`: Configured value: `true`. Description: Enables or disables basic authentication for the ERP sender channel.
    - `pr-key-alias`: Configured value: ``. Description: Private Key Alias for certificate based authentication at COD receiver channel.
    - `Host`: Configured value: `COD`. Description: Hostname for the COD receiver channel.
    - `ERP_address_1`: Configured value: `/ERP/COD/SimpleConnect`. Description: Address for the ERP sender channel.
    - `issuer`: Configured value: ``. Description: Issuer for certificate based authentication at ERP Sender channel.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found