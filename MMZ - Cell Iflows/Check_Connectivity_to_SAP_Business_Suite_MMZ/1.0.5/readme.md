**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.5

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD] --> StartEvent(Start)
    linkStyle 0 stroke:#333,stroke-width:2px,color:#333,stroke-dasharray:5 5;
    StartEvent --> Mapping
    Mapping --> EndEvent(End)
    EndEvent --> ERP[ERP]
    linkStyle 3 stroke:#333,stroke-width:2px,color:#333,stroke-dasharray:5 5;
    COD -- SOAP --> StartEvent
    EndEvent -- SOAP --> ERP
```
**BPMN Diagram**

![BPMN Diagram](./Check_Connectivity_to_SAP_Business_Suite_MMZ-1.0.5.png "BPMN Diagram")

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   COD: SOAP, EndpointSender
    -   ERP: SOAP, EndpointRecevier

-   **Key steps**
    1.  The iFlow starts with a SOAP sender adapter receiving a message from COD.
    2.  A mapping step transforms the message using the `COD_ERP_CheckEnd2EndConnectivity.opmap` mapping.
    3.  The transformed message is sent to ERP using a SOAP receiver adapter.

-   **Message transformation**
    -   `COD_ERP_CheckEnd2EndConnectivity.opmap`

-   **Externalized parameters list, configured values and their descriptions**
    -   `ERP_authentication_5`: Client Certificate
    -   `Protocol-Hostname-Port`: https\://erphost\:443
    -   `subject`: cn\=subject
    -   `artifactname`: (empty)
    -   `p-key-alias`: (empty)
    -   `ERP_allowChunking_3`: 1
    -   `issuer`: cn\=issuer
    -   `ERP_proxyType_4`: default
    -   `COD_enableBasicAuthentication_3`: true
    -   `COD_wsdlURL_1`: /wsdl/CheckConnectivityConsumer.wsdl
    -   `ERP_cleanupHeaders_2`: 1
    -   `location-id`: (empty)
    -   `Client`: 100
    -   `COD_address_2`: /COD/ERP/SimpleConnect

-   **DataStore / JMS Dependency**
    Not Found

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    Not Found

-   **ProcessDirect ComponentType Dependency**
    Not Found