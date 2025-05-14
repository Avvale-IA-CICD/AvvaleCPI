**iFlowId:** SEDA_Model_-_Single_Queue_-_Restart_and_Discard **iFlowVersion:** 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    A[SQUEUE JMS Sender] --> B{Reprocess?}
    B -- Yes --> C{Step?}
    B -- Discard --> D[Discarded Custom Status]
    D --> E[Log Discarded Message]
    E --> F[Discarded MaxRetries End]
    C -- Step1 --> G[Set Headers Step1]
    C -- Step2 --> H[Set Headers Step2]
    C -- Step3 --> I[Set Headers Step3]
    C -- Unknown --> J[Custom Status Discarded]
    J --> K[Log Discarded Message]
    K --> L[Discarded Unknown End]
    G --> M[Step 1 Process]
    H --> N[Step 2 Process]
    I --> O[Step 3 Process]
    M --> P[Next Step JMS Sender]
    N --> Q[Next Step JMS Sender]
    O --> R[Custom Status Step3Completed]
    R --> S[End Event]
    P --> T[Custom Status Step1Completed]
    T --> S
    Q --> U[Custom Status Step2Completed]
    U --> S
    W[Postman HTTPS Sender] --> X[Dummy Start Process]

```
**Functional Summary**
**Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue. It receives messages, processes them in multiple steps, and handles potential exceptions. The iFlow includes retry logic and discards messages that exceed the maximum retry attempts or are routed to an unknown step.

**Involved systems**
- SQUEUE (Sender Queue)
- RQUEUE (Receiver Queue)
- Postman

**Used Adapters**
- JMS (Sender and Receiver)
- HTTPS (Sender)

**Key steps**
1.  Receive message from SQUEUE via JMS.
2.  Determine the next processing step based on the `Step` property.
3.  Call the appropriate subprocess (Step 1, Step 2, or Step 3).
4.  Within each step, set headers and prepare the message for the subsequent step.
5.  Send the message to RQUEUE via JMS to trigger subsequent processing.
6.  If the message exceeds the maximum retries, discard it.
7.  If the step is unknown, discard it.
8.  Log exceptions to a dedicated 'Log Async Exception' sub-process.

**Message transformation**
- Enricher activities are used to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType) and message properties (Step).
- Groovy scripts (`Log_Discarded_Message.groovy`, `Log_Exception_Async.groovy`) are used for logging.
- Content modifiers are used to prepare the message body for the next step.

**Externalized parameters list and their descriptions**
- SEDA_MAIN_QUEUE: The name of the main JMS queue used for message exchange between steps.
- Number of Concurrent Processes: Number of concurrent processes for JMS.
- Maximum Retry Interval: Maximum retry interval for JMS.
- Retention Threshold 4 Alerting: Retention threshold for alerting for JMS
- Expiration Period: Expiration period for JMS
- Retry Interval: Retry Interval for JMS
- MaxRetries: Maximum number of retries before discarding a message.

**DataStore / JMS Dependency**
Yes

**Cloud Connector Dependency**
Not Found