markdown
**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    SQUEUE[SQUEUE JMS Sender] --> StartEvent[Start Event]
    StartEvent --> ReprocessGateway{Reprocess?}
    ReprocessGateway -- Discard --> DiscardedStatus[Discarded Status]
    DiscardedStatus --> LogDiscardedMessageMaxRetries[Log Discarded Message Max Retries]
    LogDiscardedMessageMaxRetries --> EndEventDiscardedMaxRetries[End Event Discarded Max Retries]
    ReprocessGateway -- Yes --> StepGateway{Step?}
    StepGateway -- Step1 --> SetHeadersStep1[Set Headers Step 1]
    SetHeadersStep1 --> Step1[Step 1 Process]
    StepGateway -- Step2 --> SetHeadersStep2[Set Headers Step 2]
    SetHeadersStep2 --> Step2[Step 2 Process]
    StepGateway -- Step3 --> SetHeadersStep3[Set Headers Step 3]
    SetHeadersStep3 --> Step3[Step 3 Process]
    StepGateway -- Unknown --> DiscardedStatusUnknown[Discarded Status Unknown]
    DiscardedStatusUnknown --> LogDiscardedMessageUnknown[Log Discarded Message Unknown]
    LogDiscardedMessageUnknown --> EndEventDiscardedUnknown[End Event Discarded Unknown]
    Step1 --> NextStep1[Next Step 1]
    Step2 --> NextStep2[Next Step 2]
    Step3 --> NextStep3[Next Step 3]
    NextStep1 --> CustomStatusStep1[Custom Status Step 1]
    NextStep2 --> CustomStatusStep2[Custom Status Step 2]
    NextStep3 --> CustomStatusStep3[Custom Status Step 3]
    CustomStatusStep1 --> EndEvent[End Event]
    CustomStatusStep2 --> EndEvent[End Event]
    CustomStatusStep3 --> EndEvent[End Event]
    NextStep1 --> RQUEUE[RQUEUE JMS Receiver]
    NextStep2 --> RQUEUE[RQUEUE JMS Receiver]
    NextStep3 --> RQUEUE[RQUEUE JMS Receiver]
    Postman[Postman HTTPS Sender] --> StartEventDummy[Start Event Dummy]
    StartEventDummy --> SetHeadersDummy[Set Headers Dummy]
    SetHeadersDummy --> SaveInitialMsg[Save Initial Msg]
    SaveInitialMsg --> CustomStatusDummy[Custom Status Dummy]
    CustomStatusDummy --> EndEventDummy[End Event Dummy]
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue. It receives messages, processes them in multiple steps (Step 1, Step 2, Step 3), and handles exceptions by logging them. The iFlow also includes logic to discard messages after a certain number of retries or if the receiver is not found.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS, EndpointSender
    - Postman: HTTPS, EndpointSender
    - RQUEUE: JMS, EndpointRecevier

- **Key steps**
    1. Receive message from SQUEUE via JMS adapter.
    2. Determine the next step based on the `Step` property.
    3. Call the corresponding process (Step 1, Step 2, or Step 3).
    4. Within each step, prepare the message for the next step and set relevant headers.
    5. Send the message to RQUEUE via JMS adapter.
    6. If the `Step` property is unknown, discard the message.
    7. If the maximum number of retries is exceeded, discard the message.
    8. Log any exceptions that occur during processing.

- **Message transformation**
    - The iFlow uses Enrichers to set headers and properties on the message.
    - Each step prepares the message for the next step by setting the `Step` property and message content.
    - Groovy scripts are used to log discarded messages and exceptions.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: The name of the JMS queue used for message processing.
    - `Number of Concurrent Processes`: Number of concurrent processes for the JMS adapter.
    - `Maximum Retry Interval`: Maximum retry interval for the JMS adapter.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting in the JMS adapter.
    - `Expiration Period`: Expiration period for messages in the JMS adapter.
    - `Retry Interval`: Retry interval for the JMS adapter.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Discarded_Message.groovy (scriptBundleId: Groovy_Logging_Scripts)
    - Log_Exception_Async.groovy (scriptBundleId: Groovy_Logging_Scripts)
    - script1.groovy (scriptBundleId: Not Found)

- **ProcessDirect ComponentType Dependency**
Not Found