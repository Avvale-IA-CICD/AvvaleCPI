**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0```mermaid
graph LR
    A[SQUEUE/Postman] --> B(StartEvent_2/StartEvent_12079806);
    B --> C{Reprocess?};
    C -- Yes --> D{Step?};
    C -- Discard --> E(Discaded);
    E --> F(Log Discarded Message);
    F --> G(Discarded MaxRetries);
    D -- Step1 --> H(Set Headers - Step1);
    D -- Step 2 --> I(Set Headers - Step2);
    D -- Step 3 --> J(Set Headers - Step3);
    D -- Unknown --> K(Custom Status - DiscardedUnknownStep);
    K --> L(Log Discarded Message - Unknown);
    L --> M(Discarded Unknown);
    H --> N(Step 1);
    I --> O(Step 2);
    J --> P(Step 3);
    N --> Q(Next Step - JMS_STEP1);
    O --> R(Next Step - JMS_STEP2);
    P --> S(Next Step - JMS_STEP3);
    Q --> T(Custom Status - Step1Completed);
    R --> U(Custom Status - Step2Completed);
    S --> V(Custom Status - Step3Completed);
    T --> W(EndEvent_2);
    U --> W;
    V --> W;
    W --> Z[RQUEUE];
    
    subgraph "Step 1 Exception"
    style AA fill:#f9f,stroke:#333,stroke-width:2px
        AA[Error Start 2] --> BB(Custom Status - Step1Exception);
        BB --> CC(Log Async Exception);
        CC --> DD[Error End 6];
    end
    
    subgraph "Step 2 Exception"
    style EE fill:#f9f,stroke:#333,stroke-width:2px
        EE[Error Start 3] --> FF(Custom Status - Step2Exception);
        FF --> GG(Log Async Exception);
        GG --> HH[Error End 3];
    end

    subgraph "Step 3 Exception"
    style II fill:#f9f,stroke:#333,stroke-width:2px
        II[Error Start 4] --> JJ(Custom Status - Step3Exception);
        JJ --> KK(Log Async Exception);
        KK --> LL[Error End 5];
    end
    
    subgraph "Router Exception"
    style MM fill:#f9f,stroke:#333,stroke-width:2px
        MM[Error Start 1] --> NN(Custom Status - RouterException);
        NN --> OO(Log Async Exception);
        OO --> PP[Error End 4];
    end
```-   **Brief description of the iFlow**

    This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using JMS queues. It receives a message, processes it through several steps (Step1, Step2, Step3), and sends it to a receiver queue. It includes error handling and retry mechanisms, with options to discard messages after a maximum number of retries or if an unknown step is encountered. The flow includes logging exceptions asynchronously.

-   **Involved systems**

    *   SQUEUE (Sender Queue)
    *   RQUEUE (Receiver Queue)
    *   Postman (HTTP Client)

-   **Used Adapters**

    *   JMS
    *   HTTPS

-   **Key steps**

    1.  Receive message from SQUEUE via JMS adapter. The message can also be initiated via HTTPS from Postman.
    2.  Set initial headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step) in "Dummy Start" process. Save the initial message to the SEDA_MAIN_QUEUE.
    3.  Route message based on the "Step" property.
    4.  Execute Step 1, Step 2, and Step 3 sub-processes sequentially. Each step prepares a message, potentially modifies it, and then progresses to the next step.
    5.  If an error occurs in any step, log the exception asynchronously and potentially discard the message.
    6.  After completing each step, a "Custom Status" enricher sets the SAP_MessageProcessingLogCustomStatus property.
    7.  Messages for processing in each Step, from Step 1 to Step 3, are sent to SEDA_MAIN_QUEUE via the JMS adapter.
    8.  If the maximum number of retries is exceeded, log and discard the message. If an unknown Step is encountered log and discard the message.

-   **Message transformation**

    *   The iFlow uses Enrichers to set headers and properties at various points in the flow.
    *   Groovy scripts are used for logging and for throwing exceptions (Test Throw Exception in Step 3).
    *   Enrichers prepare each step, Step1, Step2, Step3 by enriching the body content.

-   **Externalized parameters list and their descriptions**

    *   `SEDA_MAIN_QUEUE`: The name of the JMS queue used for the SEDA implementation.
    *   `Number of Concurrent Processes`: The number of concurrent processes for the JMS receiver adapter.
    *   `Maximum Retry Interval`: The maximum retry interval for the JMS receiver adapter.
    *   `Retry Interval`: The retry interval for the JMS receiver adapter.
    *   `MaxRetries`: The maximum number of retries before a message is discarded.
    *   `Retention Threshold 4 Alerting`: The retention threshold for alerting.
    *   `Expiration Period`: The expiration period for messages.

-   **DataStore / JMS Dependency**

    Yes