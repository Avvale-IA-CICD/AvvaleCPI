**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
```mermaid
graph LR
    A[Postman] --> B(Dummy Start)
    B --> C{Set Headers}
    C --> D[Step1 - DataStore Put]
    D --> E{Custom Status - Step 0}
    E --> F[SEDA Router Start]
    F --> G{Reprocess?}
    G -- Yes --> H{Step?}
    H -- Step1 --> I{Set Headers - Step1}
    H -- Step2 --> J{Set Headers - Step2}
    H -- Step3 --> K{Set Headers - Step3}
    H -- Unknown --> L{Custom Status - Unknown}
    I --> M[Step 1]
    J --> N[Step 2]
    K --> O[Step 3]
    M --> P[Step2 - DataStore Put]
    N --> Q[Step3 - DataStore Put]
    O --> R{Custom Status - Step3}
    P --> S{Custom Status - Step1}
    Q --> T{Custom Status - Step2}
    S --> U[SEDA Router End]
    T --> U
    R --> U
    L --> U
    G -- Discard --> V{Custom Status - DiscardedMaxRetries}
    V --> W[Log Discarded Message]
    W --> X[Discarded MaxRetries End]
    X --> U
    style U fill:#f9f,stroke:#333,stroke-width:2px
    subgraph Step1 SubProcess
    M --> PrepareStep2((Prepare Step 2))
    PrepareStep2 --> Step1End((Step1End))
    end
    subgraph Step2 SubProcess
    N --> PrepareStep3((Prepare Step 3))
    PrepareStep3 --> Step2End((Step2End))
    end
    subgraph Step3 SubProcess
    O --> TestThrowException((Test Throw Exception))
    TestThrowException --> Step3End((Step3End))
    end
```- **Brief description of the iFlow**
This iFlow processes messages retrieved from a DataStore, routes them through different processing steps (Step1, Step2, Step3) based on the 'Step' header value, and stores them back in the DataStore after each step. It also includes exception handling and logging for asynchronous exceptions. The flow handles message reprocessing and discarding based on the number of retries. An initial dummy step also receives messages via HTTPS.

- **Involved systems**
    - Postman
    - DataStore (DS)

- **Used Adapters**
    - HTTPS
    - DataStore Consumer

- **Key steps**
    1.  Receive a message via HTTPS (Dummy Start).
    2.  Store message in DataStore (Step1 in Dummy Start).
    3.  Retrieve message from DataStore (SEDA Router).
    4.  Determine the next step based on the `Step` header (SEDA Router).
    5.  Execute Step 1, Step 2, or Step 3 based on the `Step` header. Each step prepares the next step by setting a Step header and prepares the payload with a message.
    6.  Store messages in DataStore (Step2, Step3 in SEDA Router).
    7.  Set custom status in message processing log after each completed step.
    8.  Handle exceptions in each step, logging them asynchronously.
    9.  Check if maximum retries have been exceeded. If so, discard the message after logging. Otherwise, reprocess from correct step.

- **Message transformation**
    - The iFlow utilizes "Enricher" components (call activities) to set headers such as `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, and `Step`.
    - The `Prepare Step` call activities in Step 1, 2, and 3 local processes enrich the message with Base64 encoded messages, also setting the `Step` header.
    - Custom message processing log statuses are created using expressions and constants, enriching the message processing log.
    - Groovy scripts are used to log discarded messages and asynchronous exceptions.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required to access the HTTPS endpoint (Postman).
    - `Maximum Retry Interval`: Maximum interval between retries for DataStore consumption.
    - `Exponential Backoff`: Whether to use exponential backoff for DataStore retries.
    - `Data Store Name`: Name of the DataStore used for message persistence.
    - `Poll Interval`: Interval for polling the DataStore.
    - `Retry Interval`: Interval for retrying DataStore consumption.
    - `Lock Timeout`: Timeout for file locking during DataStore operations.
    - `Retention Threshold 4 Alerting`: Retention Threshold for alerting, inside the DataStore process.
    - `Expiration Period`: Expiration period for messages stored in the DataStore.
    - `MaxRetries`: The number of max retries before discarding messages.

- **DataStore / JMS Dependency**
Yes
