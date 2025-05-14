**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue. Messages are processed through multiple steps (Step 1, Step 2, Step 3), potentially retried, and ultimately either successfully processed or discarded based on the number of retries or an unknown step in the process. It includes exception handling and logging mechanisms.

- **Involved systems**
    - SQUEUE (Sender Queue)
    - RQUEUE (Receiver Queue)
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
    i.  The iFlow is triggered by a message received via HTTPS from Postman, or a JMS message from SQUEUE.
    ii. The initial message is saved. Headers are set (SAP_Sender, SAP_Receiver, SAP_MessageType).
    iii. A router checks the `Step` property and routes the message to different steps accordingly.
    iv. Each step (Step 1, Step 2, Step 3) prepares the message by enrichment, executes specific logic in respective subprocesses, potentially throwing exceptions which are logged.
    v. The message is sent to the next step via a JMS queue (RQUEUE).
    vi. If a message fails processing and the retry count (`SAPJMSRetries`) exceeds the `MaxRetries` threshold, it is discarded. Otherwise, the message is reprocessed.
    vii. Messages with an unknown step value are also discarded. Discarded messages are logged.

- **Message transformation**
    - Enrichment with headers and properties.
    - Groovy scripts for logging and potentially message modification in exception handling.
    - Transformation/preparation of messages within each "Step" process (e.g., Step1, Step2, Step3).

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the JMS queue used for message exchange between sender and receiver.
    - `Retention Threshold 4 Alerting`: Threshold for retention alerting (JMS Adapter).
    - `Expiration Period`: Expiration period for messages in the JMS queue.
    - `Number of Concurrent Processes`: Number of concurrent processes in the JMS sender adapter.
    - `Maximum Retry Interval`: Maximum interval between retries (JMS sender adapter).
    - `Retry Interval`: Interval between retries (JMS sender adapter).
    - `MaxRetries`: Maximum number of retries before a message is discarded.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman/SQUEUE] --> B(Start - HTTPS/JMS Trigger)
    B --> C{Reprocess? \n SAPJMSRetries > MaxRetries}
    C -- Yes --> D{Step?}
    C -- Discard --> E[Discarded - Max Retries \n Custom Status \n Log Discarded Message]
    E --> F((End Escalation))
    D -- Step1 --> G[Set Headers \n (Step1)]
    D -- Step2 --> H[Set Headers \n (Step2)]
    D -- Step3 --> I[Set Headers \n (Step3)]
    D -- Unknown --> J[Custom Status \n (Discarded Unknown Step) \n Log Discarded Message]
    J --> K((End Escalation))
    G --> L(Step 1 - Process)
    H --> M(Step 2 - Process)
    I --> N(Step 3 - Process)
    L --> O[Next Step \n (JMS Send - Step2)]
    M --> P[Next Step \n (JMS Send - Step3)]
    N --> Q[Next Step \n (JMS Send - EndMessage)]
    O --> R[Custom Status \n (Step1Completed)]
    P --> S[Custom Status \n (Step2Completed)]
    Q --> T[Custom Status \n (Step3Completed)]
    R --> U((End Message))
    S --> U
    T --> U
    style U fill:#f9f,stroke:#333,stroke-width:2px
    style F fill:#f9f,stroke:#333,stroke-width:2px
    style K fill:#f9f,stroke:#333,stroke-width:2px
```