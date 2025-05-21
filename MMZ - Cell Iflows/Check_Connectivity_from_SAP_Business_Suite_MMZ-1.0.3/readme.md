**iFlowId**: Check_Connectivity_from_SAP_Business_Suite_MMZ - **iFlowVersion**: 1.0

**Mermaid Diagram**
```mermaid
graph LR
    ERP[ERP] --> SOAP_ERP(StartEvent) : SOAP
    SOAP_ERP --> Mapping(Mapping)
    Mapping --> SOAP_COD(EndEvent)
    SOAP_COD --> COD[COD] : SOAP
```

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow performs an end-to-end connectivity check from SAP ERP to SAP Cloud for Customer (COD) via SAP Integration Suite.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   ERP (EndpointSender) - SOAP Adapter
    -   COD (EndpointRecevier) - SOAP Adapter

-   **Key steps**
    1.  The iFlow starts with a message from the ERP system.
    2.  A mapping step transforms the message using the `ERP_COD_ConnectivityCheck` operation mapping.
    3.  The iFlow ends by sending a message to the COD system.

-   **Message transformation**
    -   `ERP_COD_ConnectivityCheck` operation mapping is used to transform the message.

-   **Externalized parameters list, configured values and their descriptions**
    -   `COD_enableBasicAuthentication_6`: 0 (Enables/disables basic authentication for COD. 0 = disabled)
    -   `subject`:  (Subject of the certificate)
    -   `ERP_wsdlURL_0`: `/wsdl/ConnectivityCheckConsumer.wsdl` (WSDL URL for the ERP endpoint)
    -   `Port`: 443 (Port for the COD endpoint)
    -   `artifactname`:  (Name of the artifact)
    -   `ERP_enableBasicAuthentication_8`: true (Enables/disables basic authentication for ERP. true = enabled)
    -   `pr-key-alias`:  (Private key alias)
    -   `Host`: COD (Host for the COD endpoint)
    -   `ERP_address_1`: `/ERP/COD/SimpleConnect` (Address for the ERP endpoint)
    -   `issuer`:  (Issuer of the certificate)

-   **DataStore / JMS Dependency**
    Not Found

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    Not Found

-   **ProcessDirect ComponentType Dependency**
    Not Found