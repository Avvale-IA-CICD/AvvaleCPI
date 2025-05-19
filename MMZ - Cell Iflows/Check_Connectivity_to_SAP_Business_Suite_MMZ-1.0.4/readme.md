**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD] --> StartEvent(Start)
    linkStyle 0  stroke-width:2px,stroke:green;
    StartEvent --> Mapping[Mapping]
    Mapping --> EndEvent(End)
    EndEvent --> ERP[ERP]
    linkStyle 3 stroke-width:2px,stroke:green;
    COD -- SOAP --> StartEvent
    EndEvent -- SOAP --> ERP
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an End-to-End connectivity check from SAP Cloud for Customer (COD) to SAP ERP.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD (SOAP, EndpointSender)
    - ERP (SOAP, EndpointRecevier)

- **Key steps**
    1.  Receive message from COD via SOAP adapter.
    2.  Execute message mapping.
    3.  Send message to ERP via SOAP adapter.

- **Message transformation**
    - Mapping: COD_ERP_CheckEnd2EndConnectivity.opmap

- **Externalized parameters list and their descriptions**
    - COD_enableBasicAuthentication_3: Enables basic authentication for the COD sender.
    - subject: Subject for COD sender.
    - issuer: Issuer for COD sender.
    - COD_address_2: Address for the COD SOAP sender.
    - COD_wsdlURL_1: WSDL URL for the COD SOAP sender.
    - Protocol-Hostname-Port: Protocol, Hostname and Port for the ERP receiver.
    - Client: Client for the ERP receiver.
    - ERP_proxyType_4: Proxy type for the ERP receiver.
    - location-id: Location ID for the ERP receiver.
    - ERP_authentication_5: Authentication method for the ERP receiver.
    - artifactname: Credential Name for the ERP receiver.
    - ERP_allowChunking_3: Allow Chunking for the ERP receiver.
    - ERP_cleanupHeaders_2: Cleanup Headers for the ERP receiver.
    - p-key-alias: Private key alias for the ERP receiver.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found