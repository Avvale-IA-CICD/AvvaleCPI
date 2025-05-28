**iFlowId**: Testing_Endpoint - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Participant_1[Sender]
    StartEvent_2[Start]
    EndEvent_2[End]
    Participant_2[Receiver]

    Participant_1 -- HTTPS --> StartEvent_2
    StartEvent_2 --> EndEvent_2
    EndEvent_2 -- IAAdapter --> Participant_2
```
**BPMN Diagram**

![BPMN Diagram](./Testing_Endpoint-1.0.0.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**:
  This iFlow exposes an HTTPS endpoint, receives a request, and sends it to an IAAdapter endpoint.

- **Involved systems with Adapters Type and Endpoint Type**:
    - Sender: HTTPS (EndpointSender)
    - Receiver: IAAdapter (EndpointRecevier)

- **Key steps**:
    1.  Receive HTTPS request.
    2.  Send request to IAAdapter.

- **Message transformation**:
    - No message transformation steps are explicitly defined in the provided XML.

- **Externalized parameters list, configured values and their descriptions**:
    - No externalized parameters found in parameters.prop file.

- **DataStore / JMS Dependency**:
    - Not Found

- **Cloud Connector Dependency**:
    - Not Found

- **Common Scripts Dependency**:
    - Not Found

- **ProcessDirect ComponentType Dependency**:
    - Not Found