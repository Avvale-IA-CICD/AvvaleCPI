**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow processes messages read from a Data Store, performs several processing steps, and handles potential exceptions. It includes logic to retry processing, discard messages after a certain number of retries, and log exceptions. The flow is initiated either by an HTTPS call or by consuming from a DataStore.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStoreConsumer

- **Key steps**
    1.  Receive message via HTTPS or DataStore Consumer.
    2.  Store the message in a Data Store (Step1).
    3.  Retrieve the message and perform step 2 processing (Step2).
    4.  Store the message in a Data Store (Step2).
    5.  Retrieve the message and perform step 3 processing (Step3).
    6.  Store the message in a Data Store (Step3).
    7.  Add custom statuses to the message processing log to provide context during the execution.
    8. Handle exceptions by logging them.
    9. Discard messages exceeding retry limits and log this event.

- **Message transformation**
    - Several enrichers are used to set headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, and `Step`.
    - Custom statuses are added to the message processing log using expressions.
    - The content of messages is prepared and transformed using "Prepare Step" enrichers.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required for HTTPS sender authentication.
    - `Maximum Retry Interval`: Maximum interval for retry attempts.
    - `Exponential Backoff`: Flag to enable exponential backoff for retries.
    - `Data Store Name`: Name of the Data Store to be used.
    - `Poll Interval`: Interval at which the Data Store is polled.
    - `Retry Interval`: Interval between retry attempts.
    - `Lock Timeout`: Timeout for file locking mechanism in the Data Store adapter.
    - `Retention Threshold 4 Alerting`: Threshold for alerting based on data retention.
    - `Expiration Period`: Time period after which data expires.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[HTTPS or DataStore Consumer] --> B{Reprocess?}
    B -- Yes --> C{Step?}
    B -- Discard --> D[Discaded]
    C -- Step1 --> E[Set Headers (Step1)]
    C -- Step 2 --> F[Set Headers (Step2)]
    C -- Step 3 --> G[Set Headers (Step3)]
    C -- Unknown --> H[Custom Status (UnknownStep)]
    E --> I[Step 1]
    F --> J[Step 2]
    G --> K[Step 3]
    I --> L[Step2 Datastore]
    J --> M[Step3 Datastore]
    K --> N[Custom Status (Step3Completed)]
    L --> O[Custom Status (Step1Completed)]
    M --> P[Custom Status (Step2Completed)]
    O --> Q[End]
    P --> Q
    N --> Q
    H --> Q
    D --> R[Log Discarded Message]
    R --> S[Discarded MaxRetries]
    S --> T[End]
    Q --> T[End]
```
