markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman[Postman]
    DS[DS]
    StartEvent_12079784((Start Event HTTPS))
    StartEvent_2((Start Event DataStore))
    ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_12{Step?}
    CallActivity_12079790[Set Headers Step1]
    CallActivity_12079792[Set Headers Step2]
    CallActivity_12079794[Set Headers Step3]
    CallActivity_15[Step 1]
    CallActivity_18[Step 2]
    CallActivity_21[Step 3]
    CallActivity_12079780[Step2]
    CallActivity_12079781[Step3]
    CallActivity_9071[Custom Status Step1]
    CallActivity_9077[Custom Status Step2]
    CallActivity_9074[Custom Status Step3]
    CallActivity_20[Custom Status Unknown]
    CallActivity_9101[Discaded]
    CallActivity_12079775[Log Discarded Message]
    EndEvent_9104((Discarded MaxRetries))
    EndEvent_2((End))

    Postman -- HTTPS Adapter --> StartEvent_12079784
    DS -- DataStoreConsumer Adapter --> StartEvent_2
    StartEvent_2 --> ExclusiveGateway_9098
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101
    CallActivity_9101 --> CallActivity_12079775
    CallActivity_12079775 --> EndEvent_9104
    ExclusiveGateway_12 -- Step1 --> CallActivity_12079790
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079792
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079794
    ExclusiveGateway_12 -- Unknown --> CallActivity_20
    CallActivity_20 --> EndEvent_2
    CallActivity_12079790 --> CallActivity_15
    CallActivity_12079792 --> CallActivity_18
    CallActivity_12079794 --> CallActivity_21
    CallActivity_15 --> CallActivity_12079780
    CallActivity_18 --> CallActivity_12079781
    CallActivity_21 --> CallActivity_9074
    CallActivity_12079780 --> CallActivity_9071
    CallActivity_12079781 --> CallActivity_9077
    CallActivity_9071 --> EndEvent_2
    CallActivity_9077 --> EndEvent_2
    CallActivity_9074 --> EndEvent_2
```
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow processes messages retrieved from a Data Store, routes them based on the 'Step' header, and stores them back into the Data Store after each step. It includes error handling and a mechanism to discard messages that exceed a maximum retry count. The flow is initiated either by an HTTPS call or by consuming messages from a Data Store.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - DS - DataStoreConsumer - EndpointSender

- **Key steps**
 1. Receive message via HTTPS or DataStore Consumer.
 2. Check if the message should be reprocessed based on retry count. If the retry count exceeds the maximum, discard the message.
 3. Route the message based on the 'Step' header to different processing steps (Step1, Step2, Step3).
 4. Each step prepares the message, calls a local integration process, and stores the message back into the Data Store.
 5. Set custom status in message processing log after each step.
 6. Log exceptions asynchronously.

- **Message transformation**
    - The iFlow uses Enrichers to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step) with constant values.
    - Custom Status enrichers create SAP_MessageProcessingLogCustomStatus headers with expression values.
    - Step 2 and Step 1 prepare the message using enrichers to set the Step header and wrap the message content.

- **Externalized parameters list and their descriptions**
    - RoleName: User role for HTTPS sender authentication.
    - Maximum Retry Interval: Maximum interval between retries for DataStore Consumer.
    - Exponential Backoff: Flag to enable exponential backoff for DataStore Consumer retries.
    - Data Store Name: Name of the Data Store used for message persistence.
    - Poll Interval: Interval for polling messages from the Data Store.
    - Retry Interval: Interval between retries for DataStore Consumer.
    - Lock Timeout: Timeout for file lock in DataStore Consumer.
    - Retention Threshold 4 Alerting: Retention threshold for alerting in DB storage.
    - Expiration Period: Expiration period for messages in DB storage.
    - MaxRetries: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found