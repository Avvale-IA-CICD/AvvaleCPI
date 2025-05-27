**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP]
    COD[COD]
    Start[Start Event]
    Mapping[Mapping]
    End[End Event]

    ERP -- SOAP Adapter --> Start
    Start --> Mapping
    Mapping --> End
    End -- SOAP Adapter --> COD
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite (HCI).

- **Involved systems with Adapters Type and Endpoint Type**
  - ERP (EndpointSender): SOAP Adapter, HTTP Endpoint
  - COD (EndpointRecevier): SOAP Adapter, HTTP Endpoint

- **Key steps**
  1. Receive a SOAP message from ERP.
  2. Perform a message mapping (ERP_COD_ConnectivityCheck).
  3. Send a SOAP message to COD.

- **Message transformation**
  - The iFlow uses an operation mapping named `ERP_COD_ConnectivityCheck` to transform the message between ERP and COD. The mapping is located at `dir://opmap/src/main/resources/mapping/ERP_COD_ConnectivityCheck.opmap`.

- **Externalized parameters list, configured values and their descriptions**
  - `COD_enableBasicAuthentication_6`: Configured value: `0`. Description: Enables or disables basic authentication for the COD receiver channel.
  - `subject`: Configured value: ``. Description: Subject for authentication.
  - `ERP_wsdlURL_0`: Configured value: `/wsdl/ConnectivityCheckConsumer.wsdl`. Description: WSDL URL for the ERP sender channel.
  - `Port`: Configured value: `443`. Description: Port number for the COD receiver channel.
  - `artifactname`: Configured value: ``. Description: Credential name for the COD receiver channel.
  - `ERP_enableBasicAuthentication_8`: Configured value: `true`. Description: Enables or disables basic authentication for the ERP sender channel.
  - `pr-key-alias`: Configured value: ``. Description: Private key alias for the COD receiver channel.
  - `Host`: Configured value: `COD`. Description: Hostname for the COD receiver channel.
  - `ERP_address_1`: Configured value: `/ERP/COD/SimpleConnect`. Description: Address for the ERP sender channel.
  - `issuer`: Configured value: ``. Description: Issuer for authentication.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found