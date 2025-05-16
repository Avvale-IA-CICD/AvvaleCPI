**iFlowId**: Check_Connectivity_to_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    COD[COD EndpointSender] --> SOAP_COD(StartEvent)
    SOAP_COD --> Mapping
    Mapping --> SOAP_ERP(EndEvent)
    SOAP_ERP --> ERP[ERP EndpointRecevier]
    linkStyle 0,2 stroke:#333, stroke-width:2px;
    linkStyle 1 stroke:#333, stroke-width:2px;
```

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow performs an end-to-end connectivity check from SAP Cloud for Customer (COD) to SAP ERP via SAP Integration Suite.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   COD (SAP Cloud for Customer) - SOAP Adapter - EndpointSender
    -   ERP (SAP ERP) - SOAP Adapter - EndpointRecevier

-   **Key steps**
    1.  The iFlow starts upon receiving a message from COD.
    2.  The message is then passed to a mapping step.
    3.  The mapping step `COD_ERP_CheckEnd2EndConnectivity` transforms the message.
    4.  Finally, the transformed message is sent to ERP.

-   **Message transformation**
    -   Mapping `COD_ERP_CheckEnd2EndConnectivity`

-   **Externalized parameters list and their descriptions**
    -   `COD_enableBasicAuthentication_3`: Enables Basic Authentication for COD.
    -   `subject`: Subject.
    -   `issuer`: Issuer.
    -   `COD_address_2`: Address of the COD endpoint.
    -   `COD_wsdlURL_1`: WSDL URL of the COD endpoint.
    -   `Protocol-Hostname-Port`: Protocol, hostname, and port of the ERP endpoint.
    -   `Client`: Client for the ERP connection.
    -   `ERP_proxyType_4`: Proxy type for the ERP connection.
    -   `location-id`: Location ID.
    -   `ERP_authentication_5`: Authentication method for the ERP connection.
    -   `artifactname`: Credential name for the ERP connection.
    -   `ERP_allowChunking_3`: Allows chunking for the ERP connection.
    -   `ERP_cleanupHeaders_2`: Cleans up headers for the ERP connection.
    -   `p-key-alias`: Private key alias.

-   **DataStore / JMS Dependency**
    Not Found

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    Not Found

-   **ProcessDirect ComponentType Dependency**
    Not Found