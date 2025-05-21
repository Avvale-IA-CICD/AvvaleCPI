**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.3

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP EndpointSender] --> StartEvent(Start Event) -- SOAP Adapter --;
    StartEvent --> Mapping[Mapping CallActivity] -- SequenceFlow --;
    Mapping --> EndEvent(End Event) -- SequenceFlow --;
    EndEvent --> COD[COD EndpointRecevier] -- SOAP Adapter --;
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_from_SAP_Business_Suite_MMZ-1.0.3.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  Perform End2End connectivity check from SAP ERP to SAP Cloud for Customer via HCI.

- **Involved systems with Adapters Type and Endpoint Type**
    - ERP (SOAP, EndpointSender)
    - COD (SOAP, EndpointRecevier)

- **Key steps**
    1. Receive SOAP message from ERP system.
    2. Map the incoming message using Operation Mapping `ERP_COD_ConnectivityCheck`.
    3. Send SOAP message to COD system.

- **Message transformation**
    - Operation Mapping: `ERP_COD_ConnectivityCheck`

- **Externalized parameters list, configured values and their descriptions**
    - `COD_enableBasicAuthentication_6`: 0 (Enable Basic Authentication for COD)
    - `subject`:  (Subject)
    - `ERP_wsdlURL_0`: /wsdl/ConnectivityCheckConsumer.wsdl (WSDL URL for ERP)
    - `Port`: 443 (Port for COD)
    - `artifactname`:  (Credential Name for COD)
    - `ERP_enableBasicAuthentication_8`: true (Enable Basic Authentication for ERP)
    - `pr-key-alias`:  (Private Key Alias)
    - `Host`: COD (Host for COD)
    - `ERP_address_1`: /ERP/COD/SimpleConnect (Address for ERP)
    - `issuer`:  (Issuer)

- **DataStore / JMS Dependency**
    Not Found

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    Not Found

- **ProcessDirect ComponentType Dependency**
    Not Found