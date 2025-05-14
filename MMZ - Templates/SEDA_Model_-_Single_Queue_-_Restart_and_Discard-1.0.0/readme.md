**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
**Mermaid Diagram**
```mermaid
graph LR
    A[Postman] -- HTTPS --> B(Dummy Start)
    B -- JMS_STEP0 --> C[RQUEUE]
    B --> D{Reprocess?}
    D -- Discard --> E[Discaded]
    E --> F(Log Discarded Message)
    F --> G[Discarded MaxRetries]
    D -- Yes --> H{Step?}
    H -- Step1 --> I[Set Headers Step1]
    I --> J(Step 1)
    J --> K[Next Step JMS_STEP1]
    K --> L[Custom Status Step1Completed]
    L --> M[End]
    H -- Step 2 --> N[Set Headers Step2]
    N --> O(Step 2)
    O --> P[Next Step JMS_STEP2]
    P --> Q[Custom Status Step2Completed]
    Q --> M
    H -- Step 3 --> R[Set Headers Step3]
    R --> S(Step 3)
    S --> T[Custom Status Step3Completed]
    T --> M
    H -- Unknown --> U[Custom Status DiscardedUnknownStep]
    U --> V(Log Discarded Message)
    V --> W[Discarded Unknown]
    A1[SQUEUE] -- JMS_DISPATCHER --> B
    style A1 fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using JMS queues. It receives messages, processes them in a sequence of steps (Step 1, Step 2, Step 3), and sends the messages to the respective queues for each step. It handles exceptions at each step, logging them and potentially discarding messages that exceed a retry threshold or are routed to unknown destinations.

- **Involved systems**
    - SQUEUE
    - RQUEUE
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
    1. Receive message via JMS adapter from SQUEUE.
    2. Determine the next processing step based on the 'Step' property.
    3. Based on the identified step, call the related integration process: Step 1, Step 2 or Step 3.
    4. Each step updates message content.
    5. If the 'Step' property is not recognized (unknown destination), discard the message and log it.
    6. If processing fails in any step, log the exception and discard.
    7. If max retries are exceeded, discard the message and log it.
    8. After the final step has been processed, complete the iFlow.

- **Message transformation**
    - Each "Step" process (Step 1, Step 2, Step 3) contains a "Prepare Step" Enricher that modifies the message content and sets the 'Step' property for the next step.
    - "Set Headers" Enrichers add metadata (Sender, Receiver, MessageType, Status) to the message headers at different stages of the iFlow.
    - "Custom Status" Enrichers set SAP Message Processing Log Custom Statuses.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the JMS queue used for inter-step communication.
    - `Retention Threshold 4 Alerting`: Threshold for alerting on message retention.
    - `Expiration Period`: Message expiration time.
    - `Number of Concurrent Processes`: The number of concurrent processes.
    - `Maximum Retry Interval`: The maximum retry interval.
    - `Retry Interval`: The retry interval.
    - `MaxRetries`: Maximum retries before message is discarded.

- **DataStore / JMS Dependency**
Yes