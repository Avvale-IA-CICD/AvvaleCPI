**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    subgraph ERP [ERP]
        ERP_Endpoint[ERP Endpoint]
    end
    subgraph Integration_Process [Integration Process]
        Start[Start Event] --> Mapping[Mapping: ERP_COD_ConnectivityCheck]
        Mapping --> End[End Event]
    end
    subgraph COD [COD]
        COD_Endpoint[COD Endpoint]
    end

    ERP_Endpoint -- SOAP_HTTP --> Start
    End -- SOAP_HTTP --> COD_Endpoint
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (EndpointSender): SOAP Adapter (HTTP)
    - COD (EndpointRecevier): SOAP Adapter (HTTP)

- **Key steps**
 1. Receive message from ERP system via SOAP adapter.
 2. Perform mapping of the message using `ERP_COD_ConnectivityCheck.opmap`.
 3. Send message to COD system via SOAP adapter.

- **Message transformation**
    - Mapping `ERP_COD_ConnectivityCheck.opmap` is used to transform the message.

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: 0 (Enable Basic Authentication for COD)
    - `subject`: (Subject)
    - `ERP_wsdlURL_0`: /wsdl/ConnectivityCheckConsumer.wsdl (WSDL URL for ERP)
    - `Port`: 443 (Port for COD)
    - `artifactname`: (Credential Name for COD)
    - `ERP_enableBasicAuthentication_8`: true (Enable Basic Authentication for ERP)
    - `pr-key-alias`: (Private Key Alias for COD)
    - `Host`: COD (Host for COD)
    - `ERP_address_1`: /ERP/COD/SimpleConnect (Address for ERP)
    - `issuer`: (Issuer)

- **DataStore / JMS Dependency**
   Not Found

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    Not Found

- **ProcessDirect ComponentType Dependency**
    Not Found