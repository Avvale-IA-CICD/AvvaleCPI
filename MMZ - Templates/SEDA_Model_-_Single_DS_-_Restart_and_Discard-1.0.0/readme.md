**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
**Mermaid Diagram**```mermaid
graph LR
    A[Postman] -- HTTPS --> B{Start Event};
    C[DataStore] -- JDBC --> B;
    B --> D{Reprocess?};
    D -- Yes --> E{Step?};
    D -- Discard --> F[Discarded]
    F --> G[Log Discarded Message];
    G --> H(Discarded MaxRetries);
    E -- Step1 --> I[Set Headers Step1];
    E -- Step2 --> J[Set Headers Step2];
    E -- Step 3 --> K[Set Headers Step3];
    E -- Unknown --> L[Custom Status UnknownStep];
    L --> M(End Event);
    I --> N[Step 1];
    N --> O[Step2 DBStorage];
    O --> P[Custom Status Step1Completed];
    P --> M;
    J --> Q[Step 2];
    Q --> R[Step3 DBStorage];
    R --> S[Custom Status Step2Completed];
    S --> M;
    K --> T[Step 3];
    T --> U[Step3 Custom Status];
    U --> V[Custom Status Step3Completed];
    V --> M;
    style M fill:#f9f,stroke:#333,stroke-width:2px
```
**Functional Summary**- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern with a single Data Store. It includes error handling, message logging, and retry mechanisms. The iFlow receives a message, processes it through multiple steps (Step 1, Step 2, Step 3), stores the message in a Data Store at each step, and handles exceptions that may occur during processing. It also demonstrates a discard mechanism based on the maximum number of retries.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStore Consumer

- **Key steps**
    1.  Receive message via HTTPS from Postman.
    2.  Store initial message in Data Store (Step1).
    3.  Route to Step 1, Step 2, Step 3 based on header "Step".
    4.  Each step stores the message in the Data Store with "put" operation.
    5.  If a step fails, log the exception and retry.
    6.  If the maximum number of retries is exceeded, discard the message and log the event.

- **Message transformation**
    - "Set Headers" steps create or update message headers to define sender, receiver, and message type.
    - "Custom Status" steps create custom status messages for logging purposes.
    - "Prepare Step" steps enrich the message with the step's corresponding message, preparing it for storage in the Data Store.

- **Externalized parameters list and their descriptions**
    - `RoleName`: User role for HTTPS sender authentication.
    - `Maximum Retry Interval`: Maximum retry interval for DataStore consumer.
    - `Exponential Backoff`: Boolean to determine using exponential backoff for DataStore consumer.
    - `Data Store Name`: Name of the Data Store used.
    - `Poll Interval`: Poll interval for DataStore consumer.
    - `Retry Interval`: Retry interval for DataStore consumer.
    - `Lock Timeout`: Lock timeout for DataStore consumer.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting, used for Step1 and Step3 datastore operations.
    - `Expiration Period`: Expiration period for DataStore entries.
    - `MaxRetries`: Maximum number of retries before discarding message.

- **DataStore / JMS Dependency**
Yes