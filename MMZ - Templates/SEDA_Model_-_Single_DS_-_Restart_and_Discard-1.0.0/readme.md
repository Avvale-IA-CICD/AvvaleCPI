**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow demonstrates asynchronous message processing using a Data Store. It receives messages, processes them in multiple steps, and stores them in the Data Store. The flow includes error handling, logging, and a mechanism to discard messages that exceed a maximum retry count. An HTTPS endpoint triggers the flow and a DataStore Consumer receives the messages.

- **Involved systems**
    - Postman
    - DS (DataStore)

- **Used Adapters**
    - HTTPS
    - DataStore Consumer

- **Key steps**
    1.  Receive message via HTTPS from Postman
    2.  Store message in Data Store from dummy start process using DataStore Adapter
    3.  Consume message from Data Store
    4.  Route the message based on the `Step` header
    5.  Execute Step 1, Step 2, or Step 3 integration processes based on header value. Each Step has its own exception subprocess for logging.
    6.  If a `SAP_DataStoreRetries` header exists with a value that exceeds `MaxRetries`, discard the message.
    7.  Log the discarded message, and terminate the iFlow with an escalation end event.
    8.  If no issues, end the flow.

- **Message transformation**
    - The `Dummy Start` process sets initial headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType` and `Step`. It also sets a custom status.
    - Each of the `Step` integration processes sets the Step header to prepare for the called sub process.
    - The SEDA Router route the message to correct step based on the message `Step` header value.
    - Custom statuses are set at various points in the iFlow to track message processing.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Used in HTTPS adapter to determine the user role.
    - `Maximum Retry Interval`: Maximum interval for retrying message processing by the DataStoreConsumer.
    - `Exponential Backoff`: Boolean to determine if exponential backoff is used for retry mechanism in DataStoreConsumer
    - `Data Store Name`: Name of the Data Store.
    - `Poll Interval`: Interval for polling the data store.
    - `Retry Interval`: Interval between retry attempts.
    - `Lock Timeout`: Timeout for locking mechanism when retrieving the message by the DataStoreConsumer.
    - `Retention Threshold 4 Alerting`: Threshold for triggering alerts based on message retention.
    - `Expiration Period`: Time period after which the message expires in the Data Store.
    - `MaxRetries`: Maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman] -- HTTPS --> B(Dummy Start Process)
    C[DataStore] -- JDBC --> D(SEDA Router)
    B --> E{Data Store Put}
    D --> F{Reprocess ?}
    F -- Yes --> G{Step ?}
    F -- Discard --> H[Discarded Custom Status]
    H --> I[Log Discarded Message]
    I --> J((Discarded MaxRetries End Event))
    G -- Step1 --> K[Set Headers (Step1)]
    G -- Step2 --> L[Set Headers (Step2)]
    G -- Step 3 --> M[Set Headers (Step3)]
    G -- Unknown --> N[Unknown Step Custom Status]
    K --> O(Step 1)
    L --> P(Step 2)
    M --> Q(Step 3)
    N --> R((End Event))
    O --> S{Data Store Put Step2}
    P --> T{Data Store Put Step3}
    Q --> U(Step 3 Local Integration Process)
    S --> V[Step1 Custom Status]
    T --> W[Step2 Custom Status]
    U --> X[Step3 Custom Status]
    V --> R
    W --> R
    X --> R
    style J fill:#f9f,stroke:#333,stroke-width:2px
    style R fill:#ccf,stroke:#333,stroke-width:2px
```
