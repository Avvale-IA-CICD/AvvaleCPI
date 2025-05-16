**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender: HTTPS] --> StartEvent[Start]
    StartEvent --> EndEvent[End]
    EndEvent --> Receiver[Receiver: IAAdapter]
\`\`\`

**Functional Summary**
**Brief description of the iFlow**
This iFlow receives an HTTPS message, and then sends a message to an IAAdapter (AzureOpenAI).

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS, Endpoint Type: Not specified
- Receiver: IAAdapter, Endpoint Type: https://aiobs-oai-int-fc.openai.azure.com/

**Key steps**
1. Receive message via HTTPS adapter.
2. Send message to IAAdapter (AzureOpenAI).

**Message transformation**
No explicit message transformation steps are present in the provided XML. The iFlow directly connects the start and end events, implying no message content manipulation.

**Externalized parameters list and their descriptions**
- HTTPS Adapter:
    - urlPath: /test/ia
    - maximumBodySize: 40
    - userRole: ESBMessaging.send
    - senderAuthType: RoleBased
- IAAdapter:
    - ClientEndpoint: https://aiobs-oai-int-fc.openai.azure.com/
    - ChatModelID: gpt35turbo
    - ClientKey: b43cf1d6514c4d81a071274bf2237e42
    - IAType: AzureOpenAI

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found