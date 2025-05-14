**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
- **Visual representation of the flow**

```mermaid
graph LR
    A[Postman/SQUEUE] --> B(Start Event)
    B --> C{Reprocess?};
    C -- Yes --> D{Step?};
    C -- Discard --> E[Discaded];
    E --> F(Log Discarded Message);
    F --> G((Discarded MaxRetries/Unknown));
    D -- Step1 --> H[Set Headers];
    D -- Step2 --> I[Set Headers];
    D -- Step 3 --> J[Set Headers];
    D -- Unknown --> K[Custom Status];
    K --> L(Log Discarded Message);
    L --> M((Discarded Unknown));
    H --> N[Step 1];
    I --> O[Step 2];
    J --> P[Step 3];
    N --> Q[Next Step];
    O --> R[Next Step];
    P --> S[Custom Status];
    S --> T((End Event));
    Q --> U[Custom Status];
    U --> T;
    R --> V[Custom Status];
    V --> T;
```
**Functional Summary**
- **Brief description of the iFlow**
The iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue for asynchronous processing. It receives messages, processes them in multiple steps, and handles exceptions by logging them. Messages can be reprocessed or discarded based on the number of retries.

- **Involved systems**
    - SQUEUE (Sender Queue)
    - RQUEUE (Receiver Queue)
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
 1. Receives a message via HTTPS or JMS adapter.
 2. Sets headers and saves the initial message in "Dummy Start" process.
 3. Routes the message to Step 1, Step 2, or Step 3 based on the "Step" property.
 4. Each Step prepares a message and updates the "Step" property.
 5. Sends the message to the next step via JMS, or discards it if the maximum number of retries is exceeded.

- **Message transformation**
    - Enricher activities are used to create and delete message headers and properties.
    - Content modifiers are used to prepare step messages.
    - Groovy scripts are used for logging.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: Name of the JMS queue used for asynchronous communication.
    - Number of Concurrent Processes: Number of concurrent processes.
    - Maximum Retry Interval: Maximum interval for retries.
    - Expiration Period: The time after which a message expires.
    - Retention Threshold 4 Alerting: Storage retention limit to trigger an alert.
    - Retry Interval: Interval at which the message will be retried.
    - MaxRetries: Maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Yes