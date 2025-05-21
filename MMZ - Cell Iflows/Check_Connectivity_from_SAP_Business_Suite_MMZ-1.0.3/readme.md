**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.3

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] --> SOAP(StartEvent) : SOAP
    SOAP --> Mapping[Mapping]
    Mapping --> SOAP_COD(EndEvent)
    SOAP_COD --> COD[COD] : SOAP
```

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP, EndpointSender)
    - COD (SOAP, EndpointRecevier)

- **Key steps**
 1. Receive SOAP message from ERP.
 2. Perform message mapping using the `ERP_COD_ConnectivityCheck` operation mapping.
 3. Send SOAP message to COD.

- **Message transformation**
    -  `ERP_COD_ConnectivityCheck` operation mapping is used to transform the message.

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: `0` (Enable Basic Authentication for COD)
    - `subject`: `` (Subject)
    - `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (ERP WSDL URL)
    - `Port`: `443` (Port for COD)
    - `artifactname`: `` (Credential Name)
    - `ERP_enableBasicAuthentication_8`: `true` (Enable Basic Authentication for ERP)
    - `pr-key-alias`: `` (Private Key Alias)
    - `Host`: `COD` (Host for COD)
    - `ERP_address_1`: `/ERP/COD/SimpleConnect` (ERP Address)
    - `issuer`: `` (Issuer)

- **DataStore / JMS Dependency**
    Not Found

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    Not Found

- **ProcessDirect ComponentType Dependency**
    Not Found