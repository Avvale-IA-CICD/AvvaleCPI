**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    SQUEUE[SQUEUE] --> JMS_DISPATCHER(StartEvent_2) : JMS
    StartEvent_2 --> Reprocess{Reprocess?}
    Reprocess -- Yes --> StepCheck{Step?}
    Reprocess -- Discard --> DiscardMaxRetries(CallActivity_9101)
    DiscardMaxRetries --> LogDiscardedMaxRetries(CallActivity_12079775)
    LogDiscardedMaxRetries --> EndMaxRetries[EndEvent_9104]
    StepCheck -- Step1 --> SetHeaders1(CallActivity_12079818)
    StepCheck -- Step 2 --> SetHeaders2(CallActivity_12079821)
    StepCheck -- Step 3 --> SetHeaders3(CallActivity_12079824)
    StepCheck -- Unknown --> DiscardUnknown(CallActivity_20)
    DiscardUnknown --> LogDiscardedUnknown(CallActivity_12079827)
    LogDiscardedUnknown --> EndUnknown[EndEvent_12079826]
    SetHeaders1 --> Step1(CallActivity_15)
    SetHeaders2 --> Step2(CallActivity_18)
    SetHeaders3 --> Step3(CallActivity_21)
    Step1 --> NextStep1(ServiceTask_56)
    Step2 --> NextStep2(ServiceTask_9069)
    Step3 --> CustomStatus3(CallActivity_9074)
    NextStep1 --> CustomStatus1(CallActivity_9071)
    NextStep2 --> CustomStatus2(CallActivity_9077)
    CustomStatus1 --> End[EndEvent_2]
    CustomStatus2 --> End[EndEvent_2]
    CustomStatus3 --> End[EndEvent_2]
    NextStep1 --> RQUEUE : JMS
    NextStep2 --> RQUEUE : JMS
    ServiceTask_9058 --> RQUEUE : JMS
    Postman --> Start6(StartEvent_12079806) : HTTPS
    Start6 --> SetHeaders0(CallActivity_9054)
    SetHeaders0 --> SaveInitialMsg(ServiceTask_9058)
    SaveInitialMsg --> CustomStatus0(CallActivity_9062)
    CustomStatus0 --> End5[EndEvent_9052]
```

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern with a single queue. It receives messages, processes them through multiple steps, and handles exceptions. It also includes logic for retries and discarding messages that exceed the maximum retry attempts or have an unknown step.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   SQUEUE: JMS, EndpointSender
    -   Postman: HTTPS, EndpointSender
    -   RQUEUE: JMS, EndpointRecevier

-   **Key steps**
    1.  Receive message from SQUEUE via JMS adapter.
    2.  Determine the current processing step based on the `Step` property.
    3.  Based on the step, call a local integration process (Step 1, Step 2, or Step 3).
    4.  If the step is unknown, discard the message.
    5.  If the message exceeds the maximum retries, discard the message.
    6.  Log discarded messages.
    7.  Send messages to RQUEUE via JMS adapter after each step.

-   **Message transformation**
    -   The iFlow uses Enrichers to set headers and properties at various stages.
    -   The "Prepare Step" Enrichers in Step 1 and Step 2 processes modify the message body.
    -   Groovy scripts are used to log exceptions and discarded messages.

-   **Externalized parameters list and their descriptions**
    -   `SEDA_MAIN_QUEUE`: The name of the JMS queue used for message exchange.
    -   `Number of Concurrent Processes`: The number of concurrent processes for the JMS adapter.
    -   `Maximum Retry Interval`: The maximum retry interval for the JMS adapter.
    -   `Retry Interval`: The retry interval for the JMS adapter.
    -   `Use Dead Letter Queue`: Flag to use Dead Letter Queue.
    -   `ExponentialBackoff`: Flag to use Exponential Backoff.
    -   `Retention Threshold 4 Alerting`: Retention Threshold for Alerting.
    -   `Expiration Period`: Expiration Period.
    -   `MaxRetries`: Maximum number of retries before discarding the message.

-   **DataStore / JMS Dependency**
    Yes

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    -   Log_Discarded_Message.groovy, scriptBundleId: Groovy_Logging_Scripts
    -   Log_Exception_Async.groovy, scriptBundleId: Groovy_Logging_Scripts

-   **ProcessDirect ComponentType Dependency**
    Not Found