**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP EndpointSender] -->|SOAP Adapter| StartEvent(StartEvent)
    StartEvent --> Mapping(Mapping)
    Mapping --> EndEvent(EndEvent)
    EndEvent -->|SOAP Adapter| COD[COD EndpointRecevier]
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (EndpointSender) - Adapter Type: SOAP
    - COD (EndpointRecevier) - Adapter Type: SOAP

- **Key steps**
    1. Receive SOAP request from ERP system.
    2. Map the request using operation mapping ERP_COD_ConnectivityCheck.
    3. Send SOAP request to COD system.

- **Message transformation**
    - Operation Mapping: ERP_COD_ConnectivityCheck (src/main/resources/mapping/ERP_COD_ConnectivityCheck)

- **Externalized parameters list, configured values and their descriptions**
    - COD_enableBasicAuthentication_6: 0 (Enables/Disables Basic Authentication for COD receiver adapter)
    - subject:  (Subject for authentication)
    - ERP_wsdlURL_0: /wsdl/ConnectivityCheckConsumer.wsdl (WSDL URL for ERP sender adapter)
    - Port: 443 (Port for COD receiver adapter)
    - artifactname:  (Credential name for COD receiver adapter)
    - ERP_enableBasicAuthentication_8: true (Enables/Disables Basic Authentication for ERP sender adapter)
    - pr-key-alias: KeyPairCod (Private key alias for COD receiver adapter)
    - Host: COD (Host for COD receiver adapter)
    - ERP_address_1: /ERP/COD/SimpleConnect (Address for ERP sender adapter)
    - issuer:  (Issuer for authentication)

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found