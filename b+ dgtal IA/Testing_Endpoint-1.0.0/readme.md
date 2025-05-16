**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender - HTTPS Adapter] --> Start[Start Event]
    Start --> End[End Event]
    End --> Receiver[Receiver - IAAdapter]
\`\`\`

**Functional Summary**
**Brief description of the iFlow**
This iFlow receives a message via HTTPS, then sends it to Azure OpenAI using the IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS, EndpointSender
- Receiver: IAAdapter, EndpointRecevier

**Key steps**
1.  Receive message via HTTPS.
2.  Send message to Azure OpenAI using IAAdapter.

**Message transformation**
- No explicit message transformation steps are defined in the provided XML.

**Externalized parameters list and their descriptions**
-  HTTPS Adapter:
    - `urlPath`: /test/ia

-  IAAdapter Adapter:
    - `ClientKey`: b43cf1d6514c4d81a071274bf2237e42
    - `ChatModelID`: gpt35turbo
    - `ClientEndpoint`: https://aiobs-oai-int-fc.openai.azure.com/
    - `IAType`: AzureOpenAI

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found