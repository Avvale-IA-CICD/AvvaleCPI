**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    Sender[Sender] --> StartEvent[Start]
    StartEvent --> EndEvent[End]
    EndEvent --> Receiver[Receiver]
    Sender -- HTTPS --> StartEvent
    EndEvent -- IAAdapter --> Receiver
\`\`\`

**Functional Summary**
**Brief description of the iFlow**
This iFlow receives an HTTPS request, then sends it to an IAAdapter.

**Involved systems with Adapters Type and Endpoint Type**

*   Sender: HTTPS, Endpoint Type: Not Applicable
*   Receiver: IAAdapter, Endpoint Type: `https://aiobs-oai-int-fc.openai.azure.com/`

**Key steps**

1.  Receive HTTPS request from Sender.
2.  Send the message to the IAAdapter.

**Message transformation**

*   No explicit message transformation step is defined in the provided XML.

**Externalized parameters list and their descriptions**

*   No externalized parameters are explicitly defined in the provided XML.

**DataStore / JMS Dependency**

Not Found

**Cloud Connector Dependency**

Not Found

**Common Scripts Dependency**

Not Found

**ProcessDirect ComponentType Dependency**

Not Found