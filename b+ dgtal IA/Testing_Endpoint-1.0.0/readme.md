**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender] --> Start[Start Event HTTPS /test/ia]
    Start --> Process[Integration Process]
    Process --> End[End Event]
    End --> Receiver[Receiver IAAdapter]
\`\`\`

**Functional Summary**
**Brief description of the iFlow**
This iFlow receives an HTTPS request, passes it through an integration process, and then sends the message to an Azure OpenAI endpoint using the IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**
- Sender: HTTPS (RoleBased)
- Receiver: IAAdapter (AzureOpenAI - `https://aiobs-oai-int-fc.openai.azure.com/`)

**Key steps**
1. Receive HTTPS request at `/test/ia`.
2. Process message through the Integration Process.
3. Send message to the Azure OpenAI endpoint using the IAAdapter.

**Message transformation**
- No explicit message transformation steps are defined in the provided XML.

**Externalized parameters list and their descriptions**
- `urlPath`: /test/ia (Path for the HTTPS endpoint)
- `maximumBodySize`: 40 (Maximum body size for HTTPS requests)
- `ClientKey`: b43cf1d6514c4d81a071274bf2237e42 (Client key for IAAdapter)
- `ChatModelID`: gpt35turbo (Chat model ID for IAAdapter)
- `ClientEndpoint`: `https://aiobs-oai-int-fc.openai.azure.com/` (Client endpoint for IAAdapter)
- `xsrfProtection`: 0 (XSRF protection flag for HTTPS endpoint)
- `userRole`: ESBMessaging.send (User role for HTTPS endpoint)
- `senderAuthType`: RoleBased (Authentication type for HTTPS endpoint)

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
Not Found

**ProcessDirect ComponentType Dependency**
Not Found