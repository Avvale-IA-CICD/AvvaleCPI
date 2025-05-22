**iFlowId:** Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion:** 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] -->|SOAP Adapter| StartEvent((Start))
    StartEvent((Start)) --> Mapping{Mapping}
    Mapping{Mapping} --> EndEvent((End))
    EndEvent((End)) -->|SOAP Adapter| COD[COD]
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_MMZ-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an End2End connectivity check from SAP ERP to SAP Cloud for Customer via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
  - ERP (SOAP Adapter, EndpointSender)
  - COD (SOAP Adapter, EndpointRecevier)

- **Key steps**
  1.  Receive SOAP message from ERP.
  2.  Map the message using Operation Mapping `ERP_COD_ConnectivityCheck`.
  3.  Send SOAP message to COD.

- **Message transformation**
  - Operation Mapping: `ERP_COD_ConnectivityCheck`

- **Externalized parameters list, configured values and their descriptions**
  - `COD_enableBasicAuthentication_6`: `0` (Enables/disables basic authentication for COD)
  - `subject`: `` (Subject for authentication)
  - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (WSDL URL for ERP)
  - `Port`: `443` (Port for COD)
  - `artifactname`: `` (Credential name for COD authentication)
  - `ERP_enableBasicAuthentication_8`: `true` (Enables/disables basic authentication for ERP)
  - `pr-key-alias`: `` (Private key alias for COD)
  - `Host`: `COD` (Host name for COD)
  - `ERP_address_1`: `/ERP/COD/SimpleConnect` (Address for ERP)
  - `issuer`: `` (Issuer for authentication)

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found