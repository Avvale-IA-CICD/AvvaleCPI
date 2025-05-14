**iFlowId**: Hack_-_Recover_Any_Credential - **iFlowVersion**: 1.0.2

**Mermaid Diagram**
```mermaid
graph LR
    Sender1[Sender1] --> StartEvent[Start Event]
    StartEvent --> CallActivity[Call Activity: Get Credential]
    CallActivity --> EndEvent[End Event]
```
**Functional Summary**
**Brief description of the iFlow**
This iFlow retrieves a credential using a Groovy script. It receives an HTTPS request, executes a script named "RecoverAnyCredential.groovy", and then ends the flow.

**Involved systems with Adapters Type and Endpoint Type**
*   Sender1 (EndpointSender): Adapter Type: HTTPS, Endpoint Type: HTTPS

**Key steps**
1.  Receive HTTPS message.
2.  Execute Groovy script "RecoverAnyCredential.groovy".
3.  End the flow.

**Message transformation**
*   No explicit message transformation steps defined in the provided XML. The transformation logic is assumed to be handled within the RecoverAnyCredential.groovy script.

**Externalized parameters list and their descriptions**
*   No externalized parameters are explicitly defined in the provided XML. Parameters are likely handled within the Groovy script "RecoverAnyCredential.groovy".

**DataStore / JMS Dependency**
Not Found

**Cloud Connector Dependency**
Not Found

**Common Scripts Dependency**
RecoverAnyCredential.groovy

**ProcessDirect ComponentType Dependency**
Not Found