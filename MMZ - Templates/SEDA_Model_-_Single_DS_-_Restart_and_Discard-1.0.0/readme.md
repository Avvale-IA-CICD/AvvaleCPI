**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow processes messages retrieved from a Data Store (DS), routes them through a series of processing steps (Step 1, Step 2, Step 3), and stores the messages back in the Data Store after each step. It includes exception handling for each step, logging exceptions and setting custom statuses. A retry mechanism is implemented, and if the maximum number of retries is reached, the message is discarded. The process is triggered either by HTTPS call or DataStore consumption.

- **Involved systems**
    - Postman
    - DataStore (DS)

- **Used Adapters**
    - HTTPS
    - DataStore Consumer

- **Key steps**
    i.  The iFlow starts either via an HTTPS call from Postman or via DataStore consumer.
    ii. The iFlow checks if re-processing is required via "Reprocess?" gateway.
    iii.If the number of retries exceeds the defined maximum ("Discard" route), the message processing is stopped, and the message is discarded after logging.
    iv. If the number of retries is below the defined maximum ("Yes" route), the iFlow proceeds to route the message to different steps, depending on the 'Step' header value via "Step?" gateway.
    v. Each step consists of: Setting headers, calling a dedicated integration process (Step 1, Step 2, Step 3), storing the message back into the DataStore after setting custom status for the step.
    vi. If the 'Step' header does not match the predefined routes, then an 'UnknownStep' custom status is added and the process ends.
    vii.Exception subprocesses are defined at every main step, logging exception details (status and data) using "Log Async Exception" process.

- **Message transformation**
    - Header creation using enrichers (e.g., setting sender, receiver, message type, and step).
    - Custom status settings to track the message processing status at each step.
    - Payload preparation steps with enrichers.
    - Logging discarded messages by groovy script.
    - Exception details are logged using groovy script.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required to access the HTTPS endpoint.
    - `Maximum Retry Interval`: Maximum interval between retries when consuming from the Data Store.
    - `Exponential Backoff`: Boolean value indicating whether exponential backoff should be used for retries.
    - `Data Store Name`: Name of the Data Store to use.
    - `Poll Interval`: Interval at which the Data Store is polled for new messages.
    - `Retry Interval`: Interval between retries when consuming from the Data Store.
    - `Lock Timeout`: Timeout for acquiring a lock on the Data Store entry.
    - `Retention Threshold 4 Alerting`: Threshold for triggering alerts based on retention period.
    - `Expiration Period`: Period after which messages expire in the Data Store.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[HTTPS / DataStore Consumer] --> B{Reprocess?};
    B -- Yes --> C{Step?};
    B -- Discard --> D[Discaded];
    D --> E[Log Discarded Message];
    E --> F((Discarded MaxRetries));
    C -- Step1 --> G[Set Headers (Step1)];
    C -- Step2 --> H[Set Headers (Step2)];
    C -- Step3 --> I[Set Headers (Step3)];
    C -- Unknown --> J[Custom Status (UnknownStep)];
    J --> K((End));
    G --> L[Step 1];
    H --> M[Step 2];
    I --> N[Step 3];
    L --> O[Step2 DB Storage];
    O --> P[Custom Status (Step1Completed)];
    M --> Q[Step3 DB Storage];
    Q --> R[Custom Status (Step2Completed)];
    N --> S[Step3CallActivity];
    S --> T[Custom Status (Step3Completed)];
    P --> K;
    R --> K;
    T --> K;
    subgraph Step 1 Exception
    L --> LERR((Error Start 2))
    LERR --> LERR_CS[Custom Status Step1Exception]
    LERR_CS --> LERR_LOG[Log Async Exception]
    LERR_LOG --> LERR_END((Error End))
    end
    subgraph Step 2 Exception
    M --> MERR((Error Start 3))
    MERR --> MERR_CS[Custom Status Step2Exception]
    MERR_CS --> MERR_LOG[Log Async Exception]
    MERR_LOG --> MERR_END((Error End))
    end
     subgraph Step 3 Exception
    N --> NERR((Error Start 4))
    NERR --> NERR_CS[Custom Status Step3Exception]
    NERR_CS --> NERR_LOG[Log Async Exception]
    NERR_LOG --> NERR_END((Error End))
    end
```