**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
-   **Visual representation of the flow**

```mermaid
graph LR
    A[HTTPS or JMS Start] --> B{Reprocess?};
    B -- Yes --> C{Step?};
    B -- Discard --> D[Discaded: Custom Status];
    D --> E[Log Discarded Message];
    E --> F[Discarded MaxRetries End];
    C -- Step1 --> G[Set Headers Step1];
    C -- Step 2 --> H[Set Headers Step2];
    C -- Step 3 --> I[Set Headers Step3];
    C -- Unknown --> J[Custom Status DiscardedUnknownStep];
    J --> K[Log Discarded Message];
    K --> L[Discarded Unknown End];
    G --> M[Step 1];
    H --> N[Step 2];
    I --> O[Step 3];
    M --> P[Next Step Step1];
    N --> Q[Next Step Step2];
    O --> R[Custom Status Step3Completed];
    R --> S[End];
    P --> T[Custom Status Step1Completed];
    T --> S;
    Q --> U[Custom Status Step2Completed];
    U --> S;
    subgraph Step 1 Exception
    MA[Error Start Step1] --> MB[Custom Status Step1Exception];
    MB --> MC[Log Async Exception];
    MC --> MD[Error End Step1];
    end
    subgraph Step 2 Exception
    NA[Error Start Step2] --> NB[Custom Status Step2Exception];
    NB --> NC[Log Async Exception];
    NC --> ND[Error End Step2];
    end
    subgraph Step 3 Exception
    OA[Error Start Step3] --> OB[Custom Status Step3Exception];
    OB --> OC[Log Async Exception];
    OC --> OD[Error End Step3];
    end
    subgraph Common Exception Handler
    EXA[Exception Start] --> EXB[Log Async Exception];
    EXB --> EXC[Exception End];
    end
```
**Functional Summary**
-   **Brief description of the iFlow**

    This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue. It receives messages via HTTPS or JMS, processes them in three steps, and handles exceptions by logging them and potentially discarding messages after a maximum number of retries. The iFlow can be triggered via an HTTPS endpoint or from a JMS queue. It demonstrates message processing with error handling, logging, and discard mechanisms.

-   **Involved systems**

    *   SQUEUE (JMS Sender Queue)
    *   RQUEUE (JMS Receiver Queue)
    *   Postman (HTTPS Sender)

-   **Used Adapters**

    *   JMS
    *   HTTPS

-   **Key steps**

    1.  Receive message via HTTPS or JMS.
    2.  Route to subsequent steps depending on a property (Step), which will trigger local integration processes.
    3.  Each local integration process (Step 1, Step 2, Step 3) sets up the message for the next step and calls it.
    4.  If a maximum number of retries is exceeded, the message is discarded and logged.
    5.  Exceptions are caught, logged, and custom status are created.

-   **Message transformation**

    *   The iFlow uses Enrichers to set headers and properties to control routing and processing.
    *   Uses Groovy Scripts to Log exception and discarded message.
    *   Uses Enrichers for setting SAP_MessageProcessingLogCustomStatus.
    *   Payload is wrapped into Envelope.
    *   Payload is Base64 encoded.

-   **Externalized parameters list and their descriptions**

    *   SEDA_MAIN_QUEUE: The name of the main JMS queue used for message processing.
    *   Retention Threshold 4 Alerting: Retention threshold for alerting.
    *   Expiration Period: Expiration period for messages.
    *   Number of Concurrent Processes: The number of concurrent processes to use when processing messages from the JMS queue.
    *   Maximum Retry Interval: Maximum retry interval for message processing.
    *   Retry Interval: Retry interval for message processing.
    *   MaxRetries: Maximum number of retries before discarding the message.

-   **DataStore / JMS Dependency**

    Yes

-   **Cloud Connector Dependency**

    Not Found