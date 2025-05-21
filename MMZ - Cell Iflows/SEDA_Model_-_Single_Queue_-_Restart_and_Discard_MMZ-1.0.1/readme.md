**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    SQUEUE[SQUEUE] --> JMS_Dispatcher((StartEvent_2)) : JMS
    StartEvent_2 --> Reprocess{Reprocess?}
    Reprocess -- Yes --> StepDecision{Step?}
    Reprocess -- Discard --> DiscardMaxRetriesEnricher[Discarded Custom Status]

    StepDecision -- Step1 --> SetHeadersStep1[Set Headers Step 1]
    SetHeadersStep1 --> Step1[Step 1]
    Step1 --> JMSStep1((ServiceTask_56))

    StepDecision -- Step 2 --> SetHeadersStep2[Set Headers Step 2]
    SetHeadersStep2 --> Step2[Step 2]
    Step2 --> JMSStep2((ServiceTask_9069))

    StepDecision -- Step 3 --> SetHeadersStep3[Set Headers Step 3]
    SetHeadersStep3 --> Step3[Step 3]
    Step3 --> CustomStatusStep3[Custom Status Step 3]

    StepDecision -- Unknown --> DiscardUnknownEnricher[Discarded Unknown Custom Status]
    DiscardMaxRetriesEnricher --> LogDiscardedMessageMaxRetries[Log Discarded Message]
    LogDiscardedMessageMaxRetries --> DiscardMaxRetriesEnd((EndEvent_9104))

    DiscardUnknownEnricher --> LogDiscardedMessageUnknown[Log Discarded Message]
    LogDiscardedMessageUnknown --> DiscardUnknownEnd((EndEvent_12079826))

    CustomStatusStep3 --> JMSEndStep((EndEvent_2))

    JMSStep2 --> CustomStatusStep2[Custom Status]
    CustomStatusStep2 --> JMSEndStep
    JMSStep1 --> CustomStatusStep1[Custom Status]
    CustomStatusStep1 --> JMSEndStep

    JMSEndStep --> RQUEUE[RQUEUE]

```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model using a single JMS queue. It receives messages, processes them through multiple steps, and handles exceptions and retries. Messages exceeding the maximum retry count are discarded. The flow includes logging and custom status updates at various stages.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS (EndpointSender)
    - RQUEUE: JMS (EndpointRecevier)
    - Postman: HTTPS (EndpointSender)

- **Key steps**
    1. Receive message via JMS from SQUEUE.
    2. Check retry count, discard if exceeds maximum retries, else continue processing.
    3. Route message to Step 1, Step 2, or Step 3 based on the 'Step' property.
    4. Each step prepares the message, executes a direct call process, and sets a custom status.
    5. Send the message to RQUEUE.
    6. Log exceptions asynchronously if any errors occur during the steps.

- **Message transformation**
    - The iFlow uses Enrichers to set headers and properties.
    - Step 2 and Step 1 transforms with constant values for SAP_Sender, SAP_Receiver and SAP_MessageType
    - Step 3 transforms with constant values for SAP_Sender, SAP_Receiver and SAP_MessageType

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 10 - Maximum number of retries before discarding a message.
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - The name of the main JMS queue.
    - `Expiration Period`: 7 - Expiration period for messages.
    - `Maximum Retry Interval`: 1440 - Maximum interval for retries.
    - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
    - `Retry Interval`: 15 - Interval between retries.
    - `Number of Concurrent Processes`: 1 - Number of concurrent processes.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Discarded_Message.groovy, scriptBundleId: Groovy_Logging_Scripts
    - Log_Exception_Async.groovy, scriptBundleId: Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found