iFlowId: Testing_Endpoint - iFlowVersion: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender] --> StartEvent(StartEvent)
    StartEvent --> EndEvent(EndEvent)
    EndEvent --> Receiver[Receiver]
    Sender -- HTTPS Adapter --> StartEvent
    EndEvent -- IAAdapter --> Receiver
\`\`\`

**Functional Summary**
**Brief description of the iFlow**
This iFlow receives a message via HTTPS, then sends it to an Azure OpenAI endpoint using the IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS (Endpoint Type: Not applicable)
- Receiver: IAAdapter (Endpoint Type: https://aiobs-oai-int-fc.openai.azure.com/)

**Key steps**
1. Receive message via HTTPS.
2. Send message to Azure OpenAI endpoint using IAAdapter.

**Message transformation**
No explicit message transformation step is present in the provided XML.

**Externalized parameters list and their descriptions**
The following parameters are externalized for the HTTPS sender adapter:
- `urlPath`: `/test/ia`
- `maximumBodySize`: `40`

The following parameters are externalized for the IAAdapter receiver adapter:
- `ClientEndpoint`: `https://aiobs-oai-int-fc.openai.azure.com/`
- `ClientKey`: `b43cf1d6514c4d81a071274bf2237e42`
- `ChatModelID`: `gpt35turbo`
- `IAType`: `AzureOpenAI`

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found