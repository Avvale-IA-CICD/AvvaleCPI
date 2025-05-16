**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender] --> Start[Start Event HTTPS]
    Start --> End[End Event IAAdapter]
    End --> Receiver[Receiver]
    style Start fill:#f9f,stroke:#333,stroke-width:2px
    style End fill:#f9f,stroke:#333,stroke-width:2px
\`\`\`
**Functional Summary**
**Brief description of the iFlow**
This iFlow receives a message via HTTPS, and then sends it to an Azure OpenAI endpoint using the IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS, Endpoint Type: Not specified
- Receiver: IAAdapter (AzureOpenAI), Endpoint Type: https://aiobs-oai-int-fc.openai.azure.com/

**Key steps**
1. Receive message via HTTPS at the "/test/ia" endpoint.
2. Send the message to the Azure OpenAI endpoint.

**Message transformation**
No explicit message transformation steps are defined in the provided XML. The message is passed directly from the HTTPS sender to the IAAdapter receiver.

**Externalized parameters list and their descriptions**
- urlPath (HTTPS Adapter): /test/ia - The URL path for the HTTPS endpoint.
- maximumBodySize (HTTPS Adapter): 40 - The maximum body size allowed for the HTTPS request.
- ClientKey (IAAdapter): b43cf1d6514c4d81a071274bf2237e42 - The client key for the IAAdapter.
- ChatModelID (IAAdapter): gpt35turbo - The Chat Model ID for the IAAdapter.
- IAType (IAAdapter): AzureOpenAI - The IA Type for the IAAdapter.
- ClientEndpoint (IAAdapter): https://aiobs-oai-int-fc.openai.azure.com/ - The Client Endpoint for the IAAdapter.

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found