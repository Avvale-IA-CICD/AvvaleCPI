**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman -- HTTPS --> StartEvent_12079806[Start Event]
    StartEvent_12079806 --> CallActivity_9054[Set Headers]
    CallActivity_9054 --> ServiceTask_9058[Save Initial Msg]
    ServiceTask_9058 -- JMS --> RQUEUE[()]
    ServiceTask_9058 --> CallActivity_9062[Custom Status]
    CallActivity_9062 --> EndEvent_9052[End Event]
    SQUEUE -- JMS --> StartEvent_2[Start]
    StartEvent_2 --> ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12{Step?}
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
    CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
    CallActivity_12079775 --> EndEvent_9104[Discarded MaxRetries]
    ExclusiveGateway_12 -- Step1 --> CallActivity_12079818[Set Headers]
    CallActivity_12079818 --> CallActivity_15["Step 1"]
    CallActivity_15 --> ServiceTask_56[Next Step]
    ServiceTask_56 -- JMS --> RQUEUE
    ServiceTask_56 --> CallActivity_9071[Custom Status]
    CallActivity_9071 --> EndEvent_2[End]
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079821[Set Headers]
    CallActivity_12079821 --> CallActivity_18["Step 2"]
    CallActivity_18 --> ServiceTask_9069[Next Step]
    ServiceTask_9069 -- JMS --> RQUEUE
    ServiceTask_9069 --> CallActivity_9077[Custom Status]
    CallActivity_9077 --> EndEvent_2
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079824[Set Headers]
    CallActivity_12079824 --> CallActivity_21["Step 3"]
    CallActivity_21 --> CallActivity_9074[Custom Status]
    CallActivity_9074 --> EndEvent_2
    ExclusiveGateway_12 -- Unknown --> CallActivity_20[Custom Status]
    CallActivity_20 --> CallActivity_12079827[Log Discarded Message]
    CallActivity_12079827 --> EndEvent_12079826[Discarded Unknown]
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue. It receives messages, processes them through a series of steps (Step 1, Step 2, Step 3), and handles exceptions. The iFlow also includes logic to discard messages that exceed a maximum retry count or have an unknown processing step.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS, EndpointSender
    - Postman: HTTPS, EndpointSender
    - RQUEUE: JMS, EndpointRecevier

- **Key steps**
    1. Receive message from SQUEUE via JMS adapter.
    2. Determine the next processing step based on the `Step` property.
    3. Execute the corresponding step (Step 1, Step 2, or Step 3) by calling a local integration process.
    4. If the `Step` property is unknown, discard the message.
    5. If the message exceeds the maximum retry count, discard the message.
    6. Log exceptions that occur during processing.
    7. Send the message to the next queue (RQUEUE) via JMS adapter.

- **Message transformation**
    - Enricher components are used to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType) and custom status properties (SAP_MessageProcessingLogCustomStatus) at various stages of the iFlow.
    - Prepare Step activities use Enrichers to set the Step property for the next step.
    - Groovy scripts are used to log discarded messages and exceptions.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: The name of the main JMS queue used for message processing.
    - Number of Concurrent Processes: The number of concurrent processes for the JMS adapter.
    - Maximum Retry Interval: The maximum retry interval for the JMS adapter.
    - Retention Threshold 4 Alerting: Retention threshold for alerting.
    - Expiration Period: Expiration period for messages.
    - Retry Interval: Retry interval for the JMS adapter.
    - MaxRetries: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts:
        - Log_Discarded_Message.groovy
        - Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
Not Found