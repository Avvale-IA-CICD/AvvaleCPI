markdown
**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman -- HTTPS --> StartEvent_12079806(Start Event)
    SQUEUE -- JMS --> StartEvent_2(Start)
    StartEvent_2 --> ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12{Step?}
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
    CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
    CallActivity_12079775 --> EndEvent_9104(Discarded MaxRetries)
    StartEvent_12079806 --> CallActivity_9054[Set Headers]
    CallActivity_9054 --> ServiceTask_9058[Save Initial Msg]
    ServiceTask_9058 --> CallActivity_9062[Custom Status]
    CallActivity_9062 --> EndEvent_9052(End)
    ExclusiveGateway_12 -- Step1 --> CallActivity_12079818[Set Headers]
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079821[Set Headers]
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079824[Set Headers]
    ExclusiveGateway_12 -- Unknown --> CallActivity_20[Custom Status]
    CallActivity_20 --> CallActivity_12079827[Log Discarded Message]
    CallActivity_12079827 --> EndEvent_12079826(Discarded Unknown)
    CallActivity_12079818 --> CallActivity_15[Step 1]
    CallActivity_12079821 --> CallActivity_18[Step 2]
    CallActivity_12079824 --> CallActivity_21[Step 3]
    CallActivity_15 --> ServiceTask_56[Next Step]
    CallActivity_18 --> ServiceTask_9069[Next Step]
    CallActivity_21 --> CallActivity_9074[Custom Status]
    ServiceTask_56 --> CallActivity_9071[Custom Status]
    ServiceTask_9069 --> CallActivity_9077[Custom Status]
    CallActivity_9071 --> EndEvent_2(End)
    CallActivity_9077 --> EndEvent_2(End)
    CallActivity_9074 --> EndEvent_2(End)
    RQUEUE <-- JMS -- ServiceTask_56
    RQUEUE <-- JMS -- ServiceTask_9069
    
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow demonstrates a single queue SEDA (Staged Event-Driven Architecture) model with restart and discard capabilities. It receives a message, processes it through multiple steps, and sends it to a JMS queue for further processing. It includes exception handling, logging, and discarding messages exceeding the maximum retry count or messages with an unknown step.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   SQUEUE: JMS (EndpointSender)
    -   Postman: HTTPS (EndpointSender)
    -   RQUEUE: JMS (EndpointRecevier)

-   **Key steps**
    1.  Receive message via HTTPS or JMS.
    2.  Set initial headers and save the message.
    3.  Route the message to different steps (Step1, Step2, Step3) based on the `Step` property.
    4.  Each step performs specific actions (e.g., prepare step, call another integration process).
    5.  Send message to JMS queue to trigger next step.
    6.  If the `SAPJMSRetries` header exceeds `MaxRetries`, discard the message.
    7.  Log discarded messages and exceptions.

-   **Message transformation**
    -   Content Enrichers are used throughout the iFlow to set headers, properties, and message bodies.
    -   Groovy scripts are used for logging exception.
    -   The message content is wrapped and unwrapped in Base64 format within `<Envelope>` tags.

-   **Externalized parameters list, configured values and their descriptions**
    -   `MaxRetries`: 10 - Maximum number of retries before discarding the message.
    -   `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - Name of the main JMS queue used for message processing.
    -   `Expiration Period`: 7 - Expiration period for messages in days.
    -   `Maximum Retry Interval`: 1440 - Maximum retry interval in minutes.
    -   `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
    -   `Retry Interval`: 15 - Retry interval in minutes.
    -   `Number of Concurrent Processes`: 1 - Number of concurrent processes for the JMS receiver adapter.

-   **DataStore / JMS Dependency**
    Yes

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    -   Log_Discarded_Message.groovy (Groovy_Logging_Scripts)
    -   Log_Exception_Async.groovy (Groovy_Logging_Scripts)

-   **ProcessDirect ComponentType Dependency**
    Not Found