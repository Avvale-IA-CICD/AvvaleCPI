**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender: HTTPS Adapter] --> StartEvent[Start Event]
    StartEvent --> EndEvent[End Event]
    EndEvent --> Receiver[Receiver: IAAdapter]
\`\`\`
**Functional Summary**
**Brief description of the iFlow**
This iFlow receives a message via HTTPS, then it sends the message to an Azure Open AI endpoint using the IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS Adapter
- Receiver: IAAdapter - Endpoint Type: AzureOpenAI - Endpoint: https://aiobs-oai-int-fc.openai.azure.com/

**Key steps**
1.  Receive message via HTTPS.
2.  Send message to Azure OpenAI via IAAdapter.
3.  End

**Message transformation**
None.

**Externalized parameters list and their descriptions**
- HTTPS Adapter:
    - `urlPath`: `/test/ia` - URL path for the HTTPS endpoint.
    - `maximumBodySize`: `40` - Maximum body size of the message.
    - `userRole`: `ESBMessaging.send` - User role for authorization.
    - `senderAuthType`: `RoleBased` - Authentication type for the sender.

- IAAdapter:
    - `ClientKey`: `b43cf1d6514c4d81a071274bf2237e42` - Client Key for authentication.
    - `ChatModelID`: `gpt35turbo` - Chat Model ID.
    - `ClientEndpoint`: `https://aiobs-oai-int-fc.openai.azure.com/` - The endpoint for the AI service.
    - `IAType`: `AzureOpenAI` - The type of Intelligent Adapter used.

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found