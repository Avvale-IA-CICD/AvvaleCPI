**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS:::Sender StartEvent_12079806["Dummy Start"]
    StartEvent_12079806 --> CallActivity_9054["Set Headers"]
    CallActivity_9054 --> ServiceTask_9058["Save Initial Msg"]
    ServiceTask_9058 --> JMS:::Receiver RQUEUE
    CallActivity_9062["Custom Status"] --> EndEvent_9052["End"]
    ServiceTask_9058 --> CallActivity_9062["Custom Status"]
    SQUEUE --> JMS:::Sender StartEvent_2["SEDA Router Start"]
    StartEvent_2 --> ExclusiveGateway_9098["Reprocess?"]
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101["Discaded"]
    CallActivity_9101 --> CallActivity_12079775["Log Discarded Message"]
    CallActivity_12079775 --> EndEvent_9104["Discarded MaxRetries"]
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12["Step?"]
    ExclusiveGateway_12 -- Step1 --> CallActivity_12079818["Set Headers"]
    CallActivity_12079818 --> CallActivity_15["Step 1"]
    CallActivity_15 --> ServiceTask_56["Next Step"]
    ServiceTask_56 --> JMS:::Receiver RQUEUE
    ServiceTask_56 --> CallActivity_9071["Custom Status"]
    CallActivity_9071 --> EndEvent_2["SEDA Router End"]
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079821["Set Headers"]
    CallActivity_12079821 --> CallActivity_18["Step 2"]
    CallActivity_18 --> ServiceTask_9069["Next Step"]
    ServiceTask_9069 --> JMS:::Receiver RQUEUE
    ServiceTask_9069 --> CallActivity_9077["Custom Status"]
    CallActivity_9077 --> EndEvent_2["SEDA Router End"]
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079824["Set Headers"]
    CallActivity_12079824 --> CallActivity_21["Step 3"]
    CallActivity_21 --> CallActivity_9074["Custom Status"]
    CallActivity_9074 --> EndEvent_2["SEDA Router End"]
    ExclusiveGateway_12 -- Unknown --> CallActivity_20["Custom Status"]
    CallActivity_20 --> CallActivity_12079827["Log Discarded Message"]
    CallActivity_12079827 --> EndEvent_12079826["Discarded Unknown"]

    classDef Sender fill:#f9f,stroke:#333,stroke-width:2px
    classDef Receiver fill:#ccf,stroke:#333,stroke-width:2px
    classDef Process fill:#ffc,stroke:#333,stroke-width:2px
    classDef Gateway fill:#ccf,stroke:#333,stroke-width:2px

    class Postman,SQUEUE,RQUEUE Sender
    class StartEvent_2,EndEvent_2,CallActivity_15,CallActivity_18,CallActivity_21,CallActivity_12079775,CallActivity_12079827,CallActivity_12079775 Process
    class ExclusiveGateway_12,ExclusiveGateway_9098 Gateway
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model with a single queue. It receives messages, processes them through several steps, and handles exceptions, retries, and discarding of messages based on the number of retries.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS (EndpointSender)
    - RQUEUE: JMS (EndpointReceiver)
    - Postman: HTTPS (EndpointSender)

- **Key steps**
    1. Receive message via HTTPS from Postman and send to JMS queue.
    2. SEDA Router receives message from JMS queue.
    3. Depending on the 'Step' property, the message is routed to Step 1, Step 2, or Step 3.
    4. Each step (Step 1, Step 2, Step 3) processes the message.
    5. After each step, the message is sent to the next step's queue.
    6. If a step fails, the exception is logged and the message is retried.
    7. If the maximum number of retries is exceeded, the message is discarded and logged.
    8. Unknown steps are discarded.

- **Message transformation**
    - Enricher components are used to set headers and properties like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, `Step`, and `SAP_MessageProcessingLogCustomStatus`.
    - The "Prepare Step" call activities (Step 1, Step 2, Step 3) set the `Step` property and add a base64 encoded message to the body.

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 10 (Maximum number of retries before discarding a message)
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ (Name of the main JMS queue)
    - `Expiration Period`: 7 (Expiration period for messages in days)
    - `Maximum Retry Interval`: 1440 (Maximum retry interval in minutes)
    - `Retention Threshold 4 Alerting`: 1 (Retention threshold for alerting purposes)
    - `Retry Interval`: 15 (Retry interval in minutes)
    - `Number of Concurrent Processes`: 1 (Number of concurrent processes for the JMS receiver adapter)

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts: Log_Discarded_Message.groovy
    - Groovy_Logging_Scripts: Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
Not Found