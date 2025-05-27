**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_-_REPSOL - **iFlowVersion**: 1.0

**Mermaid Diagram**
- **Visual representation of the flow**

```mermaid
graph LR
    COD[COD]
    ERP[ERP]
    Start[Start]
    Mapping[Mapping]
    End[End]

    COD -- SOAP Adapter --> Start
    Start --> Mapping
    Mapping --> End
    End -- SOAP Adapter --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_-_REPSOL-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

- **Involved systems with Adapters Type and Endpoint Type**
    - COD (EndpointSender) - SOAP Adapter
    - ERP (EndpointRecevier) - SOAP Adapter

- **Key steps**
1.  The iFlow starts upon receiving a message from the COD system.
2.  A mapping step transforms the message. The mapping is defined in `src/main/resources/mapping/COD_ERP_CheckEnd2EndConnectivity`.opmap
3.  The transformed message is then sent to the ERP system.

- **Message transformation**
    - A mapping step `COD_ERP_CheckEnd2EndConnectivity.opmap` is used to transform the message between COD and ERP.

- **Externalized parameters list, configured values and their descriptions**
    - `ERP_authentication_5`: Client Certificate
    - `Protocol-Hostname-Port`: https\://erphost\:443
    - `subject`: cn\=subject
    - `artifactname`: (empty)
    - `p-key-alias`: (empty)
    - `ERP_allowChunking_3`: 1
    - `issuer`: cn\=issuer
    - `ERP_proxyType_4`: default
    - `COD_enableBasicAuthentication_3`: true
    - `COD_wsdlURL_1`: /wsdl/CheckConnectivityConsumer.wsdl
    - `ERP_cleanupHeaders_2`: 1
    - `location-id`: (empty)
    - `Client`: 100
    - `COD_address_2`: /COD/ERP/SimpleConnect

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  Not Found

- **ProcessDirect ComponentType Dependency**
  Not Found