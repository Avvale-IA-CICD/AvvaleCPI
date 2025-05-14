**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    A[Postman] --> B(Dummy Start Process)
    B --> C{Set Headers - Step 0}
    C --> D[Save Initial Msg]
    D --> E(Custom Status - Step 0)
    E --> F{JMS - RQUEUE}
    F --> G[SEDA Router Process]
    G --> H{Reprocess ?}
    H -- Yes --> I{Step ?}
    I -- Step1 --> J{Set Headers - Step 1}
    I -- Step2 --> K{Set Headers - Step 2}
    I -- Step3 --> L{Set Headers - Step 3}
    I -- Unknown --> M{Custom Status - Discarded Unknown}
    M --> N[Log Discarded Message]
    N --> O(Discarded Unknown)
    H -- Discard --> Q{Custom Status - Discaded MaxRetries}
    Q --> R[Log Discarded Message - MaxRetries]
    R --> S(Discarded MaxRetries)
    J --> P[Step 1 Process]
    P --> T{Next Step - JMS}
    T --> U(Custom Status - Step1)
    K --> V[Step 2 Process]
    V --> W{Next Step - JMS}
    W --> X(Custom Status - Step2)
    L --> Y[Step 3 Process]
    Y --> Z(Custom Status - Step3)
    Z --> AA{Next Step - JMS}
    U --> AB(End)
    X --> AB
    AA --> AB
    subgraph Step 1 Exception
    P -- Exception --> AE[Custom Status - Step1Exception]
    AE --> AF[Log Async Exception - Step 1]
    AF --> AG(Error End - Step 1)
    end
    subgraph Step 2 Exception
    V -- Exception --> AH[Custom Status - Step2Exception]
    AH --> AI[Log Async Exception - Step 2]
    AI --> AJ(Error End - Step 2)
    end
    subgraph Step 3 Exception
    Y -- Exception --> AK[Custom Status - Step3Exception]
    AK --> AL[Log Async Exception - Step 3]
    AL --> AM(Error End - Step 3)
    end
    style AB fill:#f9f,stroke:#333,stroke-width:2px
    style O fill:#f9f,stroke:#333,stroke-width:2px
    style S fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue. It receives messages, processes them in multiple steps (Step1, Step2, Step3), and handles potential exceptions during each step. Messages that fail after a configured number of retries are discarded. It includes logging and custom status updates for monitoring. The iFlow also handles initial message saving and header setting.

- **Involved systems**
    - SQUEUE
    - RQUEUE
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
    1.  Receive message via JMS from SQUEUE.
    2.  Determine the message processing Step.
    3.  If the maximum number of retries has been reached, discard the message.
    4.  Process the message in Step 1, Step 2 or Step 3 depending on the Step property
    5.  If the Step is unknown, discard the message.
    6.  Each step executes a local integration process and sets headers to prepare the next step.
    7. Log all Async Exceptions in every step.
    8. Log all Discarded Messages from unknown steps or MaxRetries step.
    9. Update the custom status to the message processing log at different steps.

- **Message transformation**
    - The iFlow uses Enrichers to set headers and properties at various stages.
    - Each step can add properties to the message.
    - The "Prepare Step" Enrichers construct XML envelopes with base64 encoded messages ("Step2Message", "Step3Message").
    - Groovy scripts "Log_Discarded_Message.groovy" and "Log_Exception_Async.groovy" are used to log discarded messages and exceptions.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: Name of the JMS queue used for message exchange.
    - Number of Concurrent Processes: Number of concurrent processes for the JMS adapter.
    - Maximum Retry Interval: The maximum retry interval for the JMS adapter.
    - Retry Interval: The retry interval for the JMS adapter.
    - Use Dead Letter Queue: Flag indicating whether to use a dead letter queue.
    - Expiration Period: Expiration period for messages in the JMS queue.
    - Retention Threshold 4 Alerting: Retention threshold for alerting purposes.
    - MaxRetries: The maximum number of retries for the message before discarding.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found