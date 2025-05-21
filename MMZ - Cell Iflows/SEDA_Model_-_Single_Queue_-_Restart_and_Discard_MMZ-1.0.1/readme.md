**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman -- HTTPS --> StartEvent_12079806(Start Event)
    StartEvent_2(Start Event) -- JMS --> ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12{Step?}
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
    CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
    CallActivity_12079775 --> EndEvent_9104(Discarded MaxRetries)
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
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model with a single queue. It receives a message, processes it through multiple steps, and handles potential exceptions. Messages exceeding the maximum retry count or with unknown steps are discarded.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS (EndpointSender)
    - RQUEUE: JMS (EndpointReceiver)
    - Postman: HTTPS (EndpointSender)

- **Key steps**
    1.  Receive message from SQUEUE via JMS adapter.
    2.  Determine the next step based on the `Step` property in the message.
    3.  Process the message through steps `Step 1`, `Step 2`, `Step 3` as required.
    4.  Log exceptions if any occur during the steps.
    5.  If `SAPJMSRetries` exceeds `MaxRetries` discard the message.
    6.  Send the processed messages to RQUEUE via JMS adapter.

- **Message transformation**
    - Enricher: Used to create/delete properties in the message body and headers to set custom message processing log statuses.
    - Groovy scripts: Used for logging exceptions and discarded messages.

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 10 - Maximum number of retries before discarding a message.
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - The name of the main JMS queue.
    - `Expiration Period`: 7 - Expiration period for messages.
    - `Maximum Retry Interval`: 1440 - Maximum retry interval in minutes.
    - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
    - `Retry Interval`: 15 - Retry interval in minutes.
    - `Number of Concurrent Processes`: 1 - The number of concurrent processes for the JMS receiver adapter.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - `Log_Exception_Async.groovy`, scriptBundleId: `Groovy_Logging_Scripts`
    - `Log_Discarded_Message.groovy`, scriptBundleId: `Groovy_Logging_Scripts`

- **ProcessDirect ComponentType Dependency**
Not Found