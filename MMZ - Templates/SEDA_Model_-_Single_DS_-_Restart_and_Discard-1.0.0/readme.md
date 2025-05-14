**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
```mermaid
graph LR
    A[Postman] --> B(Start - HTTPS)
    B --> C{Set Headers}
    C --> D[Step1 - DB Storage]
    D --> E{Custom Status - Step1}
    E --> F(Start - DataStore)
    F --> G{Reprocess?}
    G -- Yes --> H{Step?}
    H -- Step1 --> I{Set Headers - Step1}
    H -- Step2 --> J{Set Headers - Step2}
    H -- Step3 --> K{Set Headers - Step3}
    H -- Unknown --> L{Custom Status - Unknown}
    I --> M[Step1 - Process]
    J --> N[Step2 - Process]
    K --> O[Step3 - Process]
    M --> P[Step2 - DB Storage]
    N --> Q[Step3 - DB Storage]
    P --> R{Custom Status - Step2}
    Q --> S{Custom Status - Step3}
    R --> T(End)
    S --> T
    L --> T
    G -- Discard --> U{Discaded}
    U --> V[Log Discarded Message]
    V --> W(Discarded MaxRetries)
    W --> T
    subgraph Step1 - Process
        M --> M1{Prepare Step2}
        M1 --> M2(End)
        M1 -- Exception --> M3[Error Start]
        M3 --> M4{Custom Status - Step1Exception}
        M4 --> M5[Log Async Exception]
        M5 --> M6(Error End)
    end
    subgraph Step2 - Process
        N --> N1{Prepare Step3}
        N1 --> N2(End)
         N1 -- Exception --> N3[Error Start]
        N3 --> N4{Custom Status - Step2Exception}
        N4 --> N5[Log Async Exception]
        N5 --> N6(Error End)
    end
    subgraph Step3 - Process
        O --> O1[Test Throw Exception]
        O1 --> O2(End)
         O1 -- Exception --> O3[Error Start]
        O3 --> O4{Custom Status - Step3Exception}
        O4 --> O5[Log Async Exception]
        O5 --> O6(Error End)
    end
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#ccf,stroke:#333,stroke-width:2px
    style T fill:#ccf,stroke:#333,stroke-width:2px
```
- **Brief description of the iFlow**
The iFlow processes messages retrieved from a Data Store, potentially retrying failed steps. It has three main processing steps and handles exceptions by logging them. It also includes logic to discard messages that exceed the maximum retry attempts. A dummy start process provides an entry point via HTTPS for starting asynchronous processing.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStoreConsumer

- **Key steps**
    1.  Receive an HTTPS request (Dummy Start)
    2.  Store message to the datastore
    3.  Retrieve messages from the data store through SEDA Router
    4.  Execute Step 1, Step 2 and Step 3 (Local Integration Processes)
    5.  Log Async Exception (if any step fails)
    6.  Discard message (if max retries are exceeded)

- **Message transformation**
    - Set Headers (multiple enrichers to set sender, receiver, message type, and step)
    - Custom Status (multiple enrichers to set custom status in the message processing log based on message type and completed step)
    - Prepare Step (enricher to prepare the message for each processing step)
    - Groovy Scripts (for logging discarded messages and async exceptions)

- **Externalized parameters list and their descriptions**
    - RoleName: User role required to access the HTTPS endpoint.
    - Data Store Name: Name of the Data Store used for storing and retrieving messages.
    - Maximum Retry Interval: The maximum retry interval.
    - Exponential Backoff: Whether exponential backoff is enabled.
    - Poll Interval: Interval at which the Data Store is polled for new messages.
    - Retry Interval: Interval between retry attempts for failed operations.
    - Lock Timeout: Timeout for file locking mechanism used by the Data Store.
    - Retention Threshold 4 Alerting: Threshold for alerting based on data retention.
    - Expiration Period: Time after which data expires.
    - MaxRetries: The maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes