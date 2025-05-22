**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] -->|SOAP| StartEvent(Start)
    StartEvent --> Mapping(Mapping)
    Mapping --> EndEvent(End)
    EndEvent -->|SOAP| COD[COD]
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_MMZ-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from an SAP ERP system to SAP Cloud for Customer (C4C) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (EndpointSender) - SOAP Adapter
    - COD (EndpointRecevier) - SOAP Adapter

- **Key steps**
 1. The iFlow starts with a message from the ERP system.
 2. An Operation Mapping (`ERP_COD_ConnectivityCheck`) transforms the message.
 3. The transformed message is sent to the C4C system.

- **Message transformation**
    - Operation Mapping: `ERP_COD_ConnectivityCheck` (located at `src/main/resources/mapping/ERP_COD_ConnectivityCheck`)

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: `0` - Enables basic authentication for the COD receiver adapter.
    - `subject`: `` - Subject for authentication.
    - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` - WSDL URL for the ERP sender adapter.
    - `Port`: `443` - Port number for the COD receiver address.
    - `artifactname`: `` - Credential name for authentication.
    - `ERP_enableBasicAuthentication_8`: `true` - Enables basic authentication for the ERP sender adapter.
    - `pr-key-alias`: `` - Private key alias for authentication.
    - `Host`: `COD` - Hostname for the COD receiver address.
    - `ERP_address_1`: `/ERP/COD/SimpleConnect` - Address for the ERP sender adapter.
    - `issuer`: `` - Issuer for authentication.

- **DataStore / JMS Dependency**
    Not Found

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    Not Found

- **ProcessDirect ComponentType Dependency**
    Not Found