**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern using a Data Store Consumer. It picks up messages from a Data Store, processes them through a series of steps (Step 1, Step 2, Step 3), and then completes. It includes retry logic and a discard mechanism for messages that fail to process after a certain number of retries. It also incorporates exception handling at each step and asynchronous logging of exceptions. It supports triggering the flow via an HTTPS endpoint and reading messages directly from a Data Store.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStoreConsumer

- **Key steps**
    1.  Receive message via HTTPS or DataStore.
    2.  Store Message content into the DataStore (Step1).
    3.  Read Message content from the DataStore, and store it into the DataStore (Step2).
    4.  Read Message content from the DataStore, and store it into the DataStore (Step3).
    5.  Determine which route to take by checking for "Step" header.
    6.  Call Local Integration Processes - Step 1, Step 2, Step 3
    7.  Discard messages if maximum retries are exceeded.

- **Message transformation**
    - The iFlow uses Content Modifiers ("Enricher") to set headers and properties at different stages. For example, to set Sender, Receiver, and MessageType.
    - Groovy Scripts are used to log discarded messages and exceptions.
    - The "Prepare Step" Content Modifiers are used to encapsulate the "StepMessage" within XML envelop.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required to access the HTTPS endpoint.
    - `Maximum Retry Interval`: Maximum time interval between retries for the DataStoreConsumer.
    - `Exponential Backoff`: Flag to enable exponential backoff for retries in the DataStoreConsumer.
    - `Data Store Name`: Name of the Data Store used for persistence.
    - `Poll Interval`: Interval at which the DataStoreConsumer polls for new messages.
    - `Retry Interval`: Interval between retries for the DataStoreConsumer.
    - `Lock Timeout`: Timeout for file locking in the DataStoreConsumer.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting related to Step1 and Step3 DB storage.
    - `Expiration Period`: Expiration period for Step1 and Step3 DB storage.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman/DataStore] --> B{Reprocess?};
    B -- Yes --> C{Step?};
    B -- Discard --> D[Discaded];
    C -- Step1 --> E[Set Headers (Step1)];
    C -- Step2 --> F[Set Headers (Step2)];
    C -- Step 3 --> G[Set Headers (Step3)];
    C -- Unknown --> H[Custom Status (Unknown)];
    D --> I[Log Discarded Message];
    E --> J[Step 1];
    F --> K[Step 2];
    G --> L[Step 3];
    J --> M[Step2 DataStore Put];
    K --> N[Step3 DataStore Put];
    L --> O[Custom Status (Step3Completed)];
    M --> P[Custom Status (Step1Completed)];
    N --> Q[Custom Status (Step2Completed)];
    P --> R[End];
    Q --> R;
    O --> R;
    H --> R;
    I --> S[Discarded MaxRetries];
```
