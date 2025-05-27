**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP]
    COD[COD]
    StartEvent_1((Start))
    CallActivity_1[Mapping]
    EndEvent_1((End))

    ERP -- SOAP Adapter --> StartEvent_1
    StartEvent_1 --> CallActivity_1
    CallActivity_1 --> EndEvent_1
    EndEvent_1 -- SOAP Adapter --> COD
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (EndpointSender) - SOAP Adapter
    - COD (EndpointRecevier) - SOAP Adapter

- **Key steps**
 1. The iFlow starts with a message from the ERP system.
 2. A mapping step transforms the message (ERP_COD_ConnectivityCheck).
 3. The iFlow ends by sending the transformed message to the COD system.

- **Message transformation**
    - ERP_COD_ConnectivityCheck.opmap

- **Externalized parameters list, configured values and their descriptions**
    - COD_enableBasicAuthentication_6: 0 (Enable Basic Authentication on COD side)
    - subject:  (Subject for authentication)
    - ERP_wsdlURL_0: /wsdl/ConnectivityCheckConsumer.wsdl (WSDL URL for ERP)
    - Port: 443 (Port for COD)
    - artifactname:  (Credential Name)
    - ERP_enableBasicAuthentication_8: true (Enable Basic Authentication on ERP side)
    - pr-key-alias:  (Private Key Alias)
    - Host: COD (Host for COD)
    - ERP_address_1: /ERP/COD/SimpleConnect (Address for ERP)
    - issuer:  (Issuer for authentication)

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found