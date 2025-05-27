**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

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

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP Sender, EndpointSender)
    - COD (SOAP Receiver, EndpointRecevier)

- **Key steps**
    1.  Receive SOAP message from ERP.
    2.  Map the message using the `ERP_COD_ConnectivityCheck` operation mapping.
    3.  Send the mapped SOAP message to COD.

- **Message transformation**
    - The iFlow utilizes an Operation Mapping named `ERP_COD_ConnectivityCheck` located at `dir://opmap/src/main/resources/mapping/ERP_COD_ConnectivityCheck.opmap`

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: Configured value: 0. Description:  Enables/disables basic authentication for the COD receiver adapter.
    - `subject`: Configured value: . Description: Subject for certificates.
    - `ERP_wsdlURL_0`: Configured value: /wsdl/ConnectivityCheckConsumer.wsdl. Description: WSDL URL for ERP sender adapter.
    - `Port`: Configured value: 443. Description: Port number for COD.
    - `artifactname`: Configured value: . Description: Credential name for authentication to COD.
    - `ERP_enableBasicAuthentication_8`: Configured value: true. Description: Enables/disables basic authentication for the ERP sender adapter.
    - `pr-key-alias`: Configured value: . Description: Private key alias.
    - `Host`: Configured value: COD. Description: Hostname for COD.
    - `ERP_address_1`: Configured value: /ERP/COD/SimpleConnect. Description: Address for ERP sender adapter.
    - `issuer`: Configured value: . Description: Issuer for certificates.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found