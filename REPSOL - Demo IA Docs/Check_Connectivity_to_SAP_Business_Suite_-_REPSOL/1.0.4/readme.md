**iFlowId:** Check_Connectivity_to_SAP_Business_Suite_-_REPSOL - **iFlowVersion:** 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD]
    ERP[ERP]
    Start[Start Event]
    Mapping[Mapping]
    End[End Event]

    COD -- SOAP Adapter --> Start
    Start --> Mapping
    Mapping --> End
    End -- SOAP Adapter --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite (HCI/CPI).

- **Involved systems with Adapters Type and Endpoint Type**
    - COD: SOAP Adapter, EndpointSender
    - ERP: SOAP Adapter, EndpointRecevier

- **Key steps**
    1. The iFlow starts with a message from COD (SOAP Adapter).
    2. The message goes through a mapping step.
    3. The iFlow ends by sending the message to ERP (SOAP Adapter).

- **Message transformation**
    - The iFlow uses a mapping with the name "COD_ERP_CheckEnd2EndConnectivity" located at `src/main/resources/mapping/COD_ERP_CheckEnd2EndConnectivity`.

- **Externalized parameters list, configured values and their descriptions**
    - `ERP_authentication_5`: Basic (Authentication type for ERP)
    - `Protocol-Hostname-Port`: http://erphost:443 (Protocol, hostname, and port for ERP connection)
    - `subject`: cn=subject (Subject for authentication)
    - `artifactname`: EntryUserPassSAP (Credential Name for ERP Authentication)
    - `p-key-alias`: (Private Key Alias, value is empty)
    - `ERP_allowChunking_3`: 1 (Allow Chunking for ERP)
    - `issuer`: cn=issuer (Issuer for authentication)
    - `ERP_proxyType_4`: sapcc (Proxy type for ERP)
    - `COD_enableBasicAuthentication_3`: true (Enable Basic Authentication for COD)
    - `COD_wsdlURL_1`: /wsdl/CheckConnectivityConsumer.wsdl (WSDL URL for COD)
    - `ERP_cleanupHeaders_2`: 1 (Clean up Headers for ERP)
    - `location-id`: AVVALE_LID (Location ID)
    - `Client`: 100 (Client for ERP)
    - `COD_address_2`: /COD/ERP/SimpleConnect (Address for COD)

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Yes

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found