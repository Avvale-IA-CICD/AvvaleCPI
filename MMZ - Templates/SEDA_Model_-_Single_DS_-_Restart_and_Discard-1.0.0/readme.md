**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
The iFlow simulates a scenario where messages are processed asynchronously via a Data Store. It retrieves messages from a Data Store, processes them through a series of steps (Step 1, Step 2, Step 3), and handles exceptions that may occur during processing. It also includes retry logic and a discard mechanism for messages that exceed the maximum number of retries. Postman is used to start the process in the iFlow.

- **Involved systems**
    - Postman
    - DS (DataStore)

- **Used Adapters**
    - HTTPS (Sender)
    - DataStore Consumer (Sender)

- **Key steps**
    i.  An HTTPS call to `/seda/start/ds` starts the 'Dummy Start' process.
    ii. The 'Dummy Start' process sets headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step) and saves the message to a Data Store named 'Step1'.
    iii. The SEDA Router retrieves messages from the Data Store.
    iv. The Router evaluates the 'Step' header and routes the message to 'Step 1', 'Step 2', or 'Step 3' processes.
    v. If `SAP_DataStoreRetries` exceeds `MaxRetries`, the message is discarded after logging to groovy script named 'Log_Discarded_Message.groovy'.
    vi. Each "Step" process prepares its relevant step's message, and then logs any async exceptions to groovy script named 'Log_Exception_Async.groovy'.

- **Message transformation**
    - Enricher components are used to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step) with constant values.
    - Enricher components are used to set custom status messages (SAP_MessageProcessingLogCustomStatus) based on expressions.
    - Base64 Encoding used on StepXMessage.

- **Externalized parameters list and their descriptions**
    - RoleName: Role required to access the HTTPS endpoint.
    - Maximum Retry Interval: The maximum retry interval for DataStoreConsumer.
    - Exponential Backoff: Boolean value to enable exponential backoff for retry interval.
    - Data Store Name: The name of the Data Store to retrieve messages from.
    - Poll Interval: The interval at which the DataStoreConsumer polls for new messages.
    - Retry Interval: The interval between retries for DataStoreConsumer.
    - Lock Timeout: Timeout value for file locking mechanism to avoid concurrent consumption of message.
    - Retention Threshold 4 Alerting: Threshold for retention alerting for DataStore.
    - Expiration Period: Expiration period for DataStore entries.
    - MaxRetries: Maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman] --> B(HTTPS Start Event)
    B --> C{Set Headers (Initial)}
    C --> D[Data Store (Step1)]
    D --> E(Start Event - DataStoreConsumer)
    E --> F{Reprocess?}
    F -- Yes --> G{Step?}
    F -- Discard --> H[Discarded Custom Status]
    H --> I[Log Discarded Message - Groovy]
    I --> J(Discarded MaxRetries End Event)
    G -- Step1 --> K{Set Headers (Step1)}
    G -- Step2 --> L{Set Headers (Step2)}
    G -- Step3 --> M{Set Headers (Step3)}
    G -- Unknown --> N[Unknown Step Custom Status]
	N --> O(End Event)
    K --> P[Step 1 Process]
    L --> Q[Step 2 Process]
    M --> R[Step 3 Process]
    P --> S[Data Store (Step2)]
    Q --> T[Data Store (Step3)]
    S --> U[Step1 Custom Status]
	U --> O
    T --> V[Step2 Custom Status]
	V --> O
    R --> W[Step3 Custom Status]
	W --> O
    style O fill:#f9f,stroke:#333,stroke-width:2px
```
