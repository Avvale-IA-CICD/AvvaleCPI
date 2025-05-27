**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] --> SOAP_ERP(StartEvent) : SOAP Sender
    SOAP_ERP --> Mapping(Mapping)
    Mapping --> SOAP_COD(EndEvent)
    SOAP_COD --> COD[COD] : SOAP Receiver
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP Sender, EndpointSender)
    - COD (SOAP Receiver, EndpointRecevier)

- **Key steps**
1.  Receive SOAP request from ERP.
2.  Execute a mapping `ERP_COD_ConnectivityCheck` to transform the message.
3.  Send SOAP request to COD.

- **Message transformation**
    - Operation Mapping: `ERP_COD_ConnectivityCheck` located at `src/main/resources/mapping/ERP_COD_ConnectivityCheck`

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: `0` (Enables/disables basic authentication for COD)
    - `subject`: `` (Subject for some authentication purpose)
    - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (WSDL URL for ERP)
    - `Port`: `443` (Port number for COD)
    - `artifactname`: `` (Artifact Name for COD)
    - `ERP_enableBasicAuthentication_8`: `true` (Enables/disables basic authentication for ERP)
    - `pr-key-alias`: `` (Private Key Alias for COD)
    - `Host`: `COD` (Host for COD)
    - `ERP_address_1`: `/ERP/COD/SimpleConnect` (Address for ERP)
    - `issuer`: `` (Issuer for some authentication purpose)

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found