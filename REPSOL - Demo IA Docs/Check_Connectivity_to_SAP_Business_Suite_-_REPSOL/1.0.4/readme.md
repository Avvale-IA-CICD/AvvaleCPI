**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD]
    ERP[ERP]
    Start(Start)
    Mapping(Mapping)
    End(End)

    COD -- SOAP Adapter --> Start
    Start --> Mapping
    Mapping --> End
    End -- SOAP Adapter --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
  - COD (EndpointSender): SOAP Adapter
  - ERP (EndpointRecevier): SOAP Adapter

- **Key steps**
  1. The iFlow starts with a message from the COD system.
  2. A mapping step transforms the message (COD_ERP_CheckEnd2EndConnectivity.opmap).
  3. The transformed message is then sent to the ERP system.

- **Message transformation**
  The iFlow includes a mapping step that transforms the message from COD to ERP format using the `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping.

- **Externalized parameters list, configured values and their descriptions**
  - `ERP_authentication_5`: Configured Value: Client Certificate. Description: Specifies the authentication method for the ERP system.
  - `Protocol-Hostname-Port`: Configured Value: `https://erphost:443`. Description: Hostname and Port of ERP system.
  - `subject`: Configured Value: `cn=subject`. Description: Subject for certificate authentication.
  - `artifactname`: Configured Value: (empty). Description: Credential Name.
  - `p-key-alias`: Configured Value: (empty). Description: Private Key Alias.
  - `ERP_allowChunking_3`: Configured Value: `1`. Description: Flag indicating whether chunking is allowed.
  - `issuer`: Configured Value: `cn=issuer`. Description: Issuer for certificate authentication.
  - `ERP_proxyType_4`: Configured Value: `default`. Description: Proxy Type.
  - `COD_enableBasicAuthentication_3`: Configured Value: `true`. Description: Flag indicating whether basic authentication is enabled for COD.
  - `COD_wsdlURL_1`: Configured Value: `/wsdl/CheckConnectivityConsumer.wsdl`. Description: WSDL URL for COD.
  - `ERP_cleanupHeaders_2`: Configured Value: `1`. Description: Flag indicating whether headers should be cleaned up.
  - `location-id`: Configured Value: (empty). Description: Location ID.
  - `Client`: Configured Value: `100`. Description: Client ID for ERP.
  - `COD_address_2`: Configured Value: `/COD/ERP/SimpleConnect`. Description: Address for COD.

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found