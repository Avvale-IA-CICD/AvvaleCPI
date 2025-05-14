**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    subgraph Start_iFlow [Start iFlow]
        Postman --> HTTPS_StartEvent
    end
    
    subgraph Step0_Process [Step 0 Process]
        HTTPS_StartEvent --> SetHeadersStep0
        SetHeadersStep0 --> SaveInitialMsg
        SaveInitialMsg --> JMS_STEP0_Send[Send JMS_STEP0 to RQUEUE]
        JMS_STEP0_Send --> CustomStatusStep0
        CustomStatusStep0 --> EndEventStep0
    end

    subgraph SEDA_Router [SEDA Router]
        SQUEUE --> StartEventSEDA
        StartEventSEDA --> ReprocessCheck{Reprocess?}
        ReprocessCheck -- Discard --> DiscardActivity
        DiscardActivity --> LogDiscardedMessageMaxRetries
        LogDiscardedMessageMaxRetries --> EndEventDiscardedMaxRetries

        ReprocessCheck -- Yes --> StepCheck{Step?}
        StepCheck -- Step1 --> SetHeadersStep1
        SetHeadersStep1 --> Step1Call[[Step 1]]
        Step1Call --> NextStep1[Next Step]
        NextStep1 --> CustomStatusStep1
        CustomStatusStep1 --> EndEventNextStep1

        StepCheck -- Step2 --> SetHeadersStep2
        SetHeadersStep2 --> Step2Call[[Step 2]]
        Step2Call --> NextStep2[Next Step]
        NextStep2 --> CustomStatusStep2
        CustomStatusStep2 --> EndEventNextStep2

        StepCheck -- Step3 --> SetHeadersStep3
        SetHeadersStep3 --> Step3Call[[Step 3]]
        Step3Call --> CustomStatusStep3
        CustomStatusStep3 --> EndEventNextStep3

        StepCheck -- Unknown --> CustomStatusUnknown
        CustomStatusUnknown --> LogDiscardedMessageUnknown
        LogDiscardedMessageUnknown --> EndEventDiscardedUnknown

        EndEventNextStep1 --> EndEventSEDA
        EndEventNextStep2 --> EndEventSEDA
        EndEventNextStep3 --> EndEventSEDA
        EndEventDiscardedMaxRetries --> EndEventSEDA
        EndEventDiscardedUnknown --> EndEventSEDA
    end

    subgraph Step1 [Step 1]
        SetHeadersStep1 --> PrepareStep2
        PrepareStep2 --> EndEventStep1
    end

    subgraph Step2 [Step 2]
        SetHeadersStep2 --> PrepareStep3
        PrepareStep3 --> EndEventStep2
    end

    subgraph Step3 [Step 3]
        SetHeadersStep3 --> TestThrowException
        TestThrowException --> EndEventStep3
    end

    subgraph LogAsync [Log Async Exception]
        StartEventLogAsync --> LogAsyncExceptionActivity
        LogAsyncExceptionActivity --> EndEventLogAsync
    end

    subgraph ErrorSubProcess [ErrorSubProcess]
        StartErrorEvent --> CustomStatusError
        CustomStatusError --> LogAsyncException
        LogAsyncException --> EndErrorEvent
    end

    RQUEUE --> StartEventSEDA

    classDef subprocess fill:#f9f,stroke:#333,stroke-width:2px
    class Step1, Step2, Step3, LogAsync, ErrorSubProcess subprocess
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern with a single queue. It simulates message processing across multiple steps, handling exceptions and discarding messages that exceed retry limits or encounter unknown steps. It allows for message restart and implements a discard mechanism.

- **Involved systems**
    - SQUEUE (Sender Queue)
    - RQUEUE (Receiver Queue)
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
 1. Receives a message via HTTPS endpoint.
 2. Initializes message headers and saves the message to JMS queue.
 3. SEDA Router:
    - Checks for max retries and discards the message if needed.
    - Routes the message based on the `Step` property:
        - Step1: Calls "Step 1" Integration Process.
        - Step2: Calls "Step 2" Integration Process.
        - Step3: Calls "Step 3" Integration Process.
        - Unknown Step: Discards the message.
 4. Each "Step" Integration Process performs some logic (simulated by setting properties and custom statuses). These step processes call a Groovy script activity where exceptions can be thrown. Exceptions are handled via error sub-processes, then logged and rethrown.
 5. The `Next Step` service task sends the message to the JMS Queue.
 6. After Step3 is completed, the iFlow ends.
 7. When discarding a message because max retries is reached, the discard activity will be performed.
 8. The `Log Async Exception` Integration Process is called in any of the exception sub-processes to log exceptions that occur during the flow.

- **Message transformation**
    - Enricher: Used to set headers and properties for routing and logging.
    - Groovy Scripts: Used for custom logging and simulating exceptions.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the main JMS queue used for message processing.
    - `Number of Concurrent Processes`: Number of concurrent processes.
    - `Maximum Retry Interval`: Maximum retry interval in milliseconds.
    - `Retry Interval`: Retry interval in milliseconds.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting.
    - `Expiration Period`: Expiration period for messages.
    - `MaxRetries`: Maximum number of retries allowed before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found
