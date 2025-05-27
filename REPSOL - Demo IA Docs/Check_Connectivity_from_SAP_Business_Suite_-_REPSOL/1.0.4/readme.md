**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP]
    COD[COD]
    StartEvent[Start Event]
    Mapping[Mapping]
    EndEvent[End Event]

    ERP -- SOAP --> StartEvent
    StartEvent --> Mapping
    Mapping --> EndEvent
    EndEvent -- SOAP --> COD
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
  - ERP (SOAP, EndpointSender)
  - COD (SOAP, EndpointRecevier)

- **Key steps**
  1. Receive a message from ERP via SOAP adapter.
  2. Perform a mapping using `ERP_COD_ConnectivityCheck.opmap`.
  3. Send the message to COD via SOAP adapter.

- **Message transformation**
  -  The iFlow uses the `ERP_COD_ConnectivityCheck.opmap` operation mapping to transform the message.

- **Externalized parameters list, configured values and their descriptions**
  - `COD_enableBasicAuthentication_6`: 0 (Enables/Disables Basic Authentication for the COD receiver SOAP adapter)
  - `subject`:  (Subject for authentication)
  - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (WSDL URL for the ERP sender SOAP adapter)
  - `Port`: `443` (Port number for the COD receiver SOAP adapter)
  - `artifactname`:  (Credential name for the COD receiver SOAP adapter)
  - `ERP_enableBasicAuthentication_8`: `true` (Enables/Disables Basic Authentication for the ERP sender SOAP adapter)
  - `pr-key-alias`:  (Private key alias for the COD receiver SOAP adapter)
  - `Host`: `COD` (Host name for the COD receiver SOAP adapter)
  - `ERP_address_1`: `/ERP/COD/SimpleConnect` (Address for the ERP sender SOAP adapter)
  - `issuer`:  (Issuer for authentication)

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found