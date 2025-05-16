**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.3

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP EndpointSender] --> SOAP_ERP(StartEvent SOAP Adapter)
    SOAP_ERP --> Mapping[Mapping]
    Mapping --> SOAP_COD(EndEvent SOAP Adapter)
    SOAP_COD --> COD[COD EndpointRecevier]
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP Adapter, EndpointSender)
    - COD (SOAP Adapter, EndpointRecevier)

- **Key steps**
    1.  Receive a message from ERP via SOAP adapter.
    2.  Perform a mapping using the `ERP_COD_ConnectivityCheck` operation mapping.
    3.  Send the transformed message to COD via SOAP adapter.

- **Message transformation**
    - The iFlow uses the `ERP_COD_ConnectivityCheck` operation mapping to transform the message.

- **Externalized parameters list and their descriptions**
    - `ERP_enableBasicAuthentication_8`: Enables basic authentication for the ERP sender adapter.
    - `subject`: Subject for ERP.
    - `issuer`: Issuer for ERP.
    - `ERP_address_1`: Address of the ERP SOAP sender adapter.
    - `ERP_wsdlURL_0`: WSDL URL of the ERP SOAP sender adapter.
    - `Host`: Hostname for COD.
    - `Port`: Port for COD.
    - `COD_enableBasicAuthentication_6`: Enables basic authentication for the COD receiver adapter.
    - `artifactname`: Credential name for COD.
    - `pr-key-alias`: Private key alias for COD.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found