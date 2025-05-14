**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
The iFlow processes messages retrieved from a Data Store, routes them based on a 'Step' header to different processing steps (Step 1, Step 2, Step 3), and handles exceptions. It includes logic to discard messages that exceed a maximum retry count. The iFlow leverages direct calls to local integration processes for each step, enriching messages with custom status updates and logging exceptions asynchronously. It also uses a SEDA router to handle the message flow and exception handling.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStoreConsumer

- **Key steps**
    1. Receive HTTPS request from Postman to trigger the process via 'Dummy Start' process.
    2. 'Dummy Start' sets headers, saves the message to the Data Store using the "Step1" name.
    3. SEDA Router receives message from Data Store Consumer adapter.
    4. SEDA Router checks if message re-processing is needed.
    5. SEDA Router routes based on 'Step' header (Step1, Step2, Step3).
    6. Each Step prepares the message and enrich headers and wraps the message to call the next direct process.
    7. The steps save the message to the Data Store with the matching Step name.
    8. Each Step has an exception subprocess to log exceptions.
    9. After all steps are complete, the message processing log is updated with a success status, using the Step name and the message is completed.
    10. Messages exceeding 'MaxRetries' are discarded and logged.
    11. "Test Throw Exception" throws an exception from a groovy script that is catched to log exceptions asynchronously.

- **Message transformation**
    - Set Headers: Multiple steps to create or update headers.
    - Prepare Step 2/3: Enricher to prepare the message for the called process.
    - Custom Status: Enricher activities that set the SAP_MessageProcessingLogCustomStatus header.

- **Externalized parameters list and their descriptions**
    - `RoleName`: (HTTPS Adapter) User role required to access the endpoint.
    - `Maximum Retry Interval`: (DataStoreConsumer Adapter) Maximum time interval between retry attempts.
    - `Exponential Backoff`: (DataStoreConsumer Adapter) Flag indicating whether exponential backoff is enabled for retry attempts.
    - `Data Store Name`: (DataStoreConsumer & DBstorage Adapter) Name of the Data Store used for persistence.
    - `Poll Interval`: (DataStoreConsumer Adapter) Interval between polls to the Data Store.
    - `Retry Interval`: (DataStoreConsumer Adapter) Interval between retry attempts.
    - `Lock Timeout`: (DataStoreConsumer Adapter) Timeout for file locking.
    - `Retention Threshold 4 Alerting`: (DBstorage Adapter) Retention threshold for alerting.
    - `Expiration Period`: (DBstorage Adapter) Expiration period for data in the Data Store.
    - `MaxRetries`: (Exclusive Gateway Reprocess?) Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman] --> B(HTTPS Start Event);
    C(Data Store Consumer Start Event) --> D{Reprocess?};
    B --> E[Set Headers (Step 0)];
    E --> F[Data Store PUT (Step 1)];
    F --> G[Custom Status (Step 0 Completed)];
    G --> H(End Event);
    D -- Yes --> I{Step?};
    D -- Discard --> J[Custom Status (DiscardedMaxRetries)];
    J --> K[Log Discarded Message];
    K --> L(Discarded MaxRetries End Event);
    I -- Step1 --> M[Set Headers (Step 1)];
    I -- Step2 --> N[Set Headers (Step 2)];
    I -- Step 3 --> O[Set Headers (Step 3)];
    I -- Unknown --> P[Custom Status (UnknownStep)];
    P --> H;
    M --> Q[Step 1 Process];
    N --> R[Step 2 Process];
    O --> S[Step 3 Process];
    Q --> T[Data Store PUT (Step 2)];
    R --> U[Data Store PUT (Step 3)];
    S --> V[Step 3 Custom Status];
    T --> W[Step 1 Custom Status];
    U --> X[Step 2 Custom Status];
    W --> H;
    X --> H;
    V --> H;
    style H fill:#f9f,stroke:#333,stroke-width:2px
```
