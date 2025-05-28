**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Participant_1[Sender] -- HTTPS --> StartEvent_2((Start))
    StartEvent_2((Start)) --> EndEvent_2((End))
    EndEvent_2((End)) -- IAAdapter --> Participant_2[Receiver]
```
**BPMN Diagram**

![BPMN Diagram](./Testing_Endpoint-1.0.0.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow exposes an HTTPS endpoint, receives a message, and sends it to an IAAdapter endpoint.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender: HTTPS (EndpointSender)
    - Receiver: IAAdapter (EndpointRecevier)

- **Key steps**
    1. Receive message via HTTPS.
    2. Send message to IAAdapter.

- **Message transformation**
    - None

- **Externalized parameters list, configured values and their descriptions**
    - None

- **DataStore / JMS Dependency**
    Not Found

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    Not Found

- **ProcessDirect ComponentType Dependency**
    Not Found