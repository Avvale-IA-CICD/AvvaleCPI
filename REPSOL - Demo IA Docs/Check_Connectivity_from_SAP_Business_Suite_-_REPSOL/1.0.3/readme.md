**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP]
    COD[COD]
    StartEvent[Start Event]
    Mapping[Mapping]
    EndEvent[End Event]

    ERP -- SOAP Adapter --> StartEvent
    StartEvent --> Mapping
    Mapping --> EndEvent
    EndEvent -- SOAP Adapter --> COD
    style ERP fill:#f9f,stroke:#333,stroke-width:2px
    style COD fill:#f9f,stroke:#333,stroke-width:2px
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
 This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
  - ERP (EndpointSender) - Adapter Type: SOAP
  - COD (EndpointRecevier) - Adapter Type: SOAP

- **Key steps**
 1.  The iFlow starts with a SOAP message received from ERP.
 2.  A mapping step transforms the message using `ERP_COD_ConnectivityCheck.opmap`.
 3.  The transformed message is then sent to COD via SOAP adapter.

- **Message transformation**
  - `ERP_COD_ConnectivityCheck.opmap` is used for mapping.

- **Externalized parameters list, configured values and their descriptions**
  - `COD_enableBasicAuthentication_6`: 0 (Enables/disables basic authentication for COD)
  - `subject`:  (Subject for authentication)
  - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (WSDL URL for ERP)
  - `Port`: 443 (Port for COD)
  - `artifactname`:  (Credential name for authentication)
  - `ERP_enableBasicAuthentication_8`: true (Enables/disables basic authentication for ERP)
  - `pr-key-alias`:  (Private key alias)
  - `Host`: `COD` (Host address for COD)
  - `ERP_address_1`: `/ERP/COD/SimpleConnect` (Address for ERP)
  - `issuer`:  (Issuer for authentication)

- **DataStore / JMS Dependency**
 Not Found

- **Cloud Connector Dependency**
 Not Found

- **Common Scripts Dependency**
 Not Found

- **ProcessDirect ComponentType Dependency**
 Not Found