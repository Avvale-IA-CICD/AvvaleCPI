**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
```mermaid
graph LR
    A[Postman] -- HTTPS --> B(Start Event)
    B --> C{Set Headers (Step 0)}
    C --> D[Save Initial Msg]
    D --> E{Custom Status (Step 0 Completed)}
    E --> F(JMS_STEP0)
    F --> G((SQUEUE))
    G --> H(Start)
    H --> I{Reprocess?}
    I -- Yes --> J{Step?}
    I -- Discard --> K[Discaded]
    K --> L[Log Discarded Message (Max Retries)]
    L --> M((Discarded MaxRetries))

    J -- Step1 --> N{Set Headers (Step 1)}
    N --> O[Step 1]
    O --> P(Next Step (Step2))
    P --> Q{Custom Status (Step 1 Completed)}
    Q --> R(JMS_STEP2)
    R --> G

    J -- Step 2 --> S{Set Headers (Step 2)}
    S --> T[Step 2]
    T --> U(Next Step (Step3))
    U --> V{Custom Status (Step 2 Completed)}
    V --> W(JMS_STEP3)
    W --> G

    J -- Step 3 --> X{Set Headers (Step 3)}
    X --> Y[Step 3]
    Y --> Z{Custom Status (Step 3 Completed)}
    Z --> AA((End))

    J -- Unknown --> BB[Custom Status (Discarded Unknown)]
    BB --> CC[Log Discarded Message (Unknown)]
    CC --> DD((Discarded Unknown))
```
- **Brief description of the iFlow**
The iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue. It receives messages, processes them in multiple steps (Step 1, Step 2, Step 3), and logs exceptions. The iFlow also handles message retries and discards messages that exceed the maximum retry attempts or encounter unknown routing steps.

- **Involved systems**
    - SQUEUE (Source JMS Queue)
    - RQUEUE (Target JMS Queue)
    - Postman (HTTP Sender)

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
 1. Receive message from SQUEUE via JMS adapter.
 2. Determine the processing step based on message properties.
 3. Call subprocesses Step 1, Step 2, or Step 3 based on the Step property.
 4. Within each step, prepare the message, potentially throw an exception and enrich the log.
 5. If the `SAPJMSRetries` header exceeds a threshold, discard the message.
 6. Log discarded messages and exceptions.
 7. Send the message to the next step via the JMS adapter to RQUEUE.
 8. Complete or discard the message.

- **Message transformation**
    - Header enrichment to set sender, receiver, and message type.
    - Body enrichment to prepare each step with a specific message using a constant and wrapped in `<Envelope>` XML tags.
    - Status enrichment to set `SAP_MessageProcessingLogCustomStatus` for each step.
    - Groovy scripts are used to log exceptions and discarded messages.

- **Externalized parameters list and their descriptions**
    - `{{SEDA_MAIN_QUEUE}}`: Name of the main JMS queue used for message exchange.
    - `{{Number of Concurrent Processes}}`: Specifies the number of concurrent processes for the JMS sender adapter.
    - `{{Maximum Retry Interval}}`: Maximum retry interval for the JMS adapter.
    - `{{Retry Interval}}`: Retry interval for the JMS adapter.
    - `{{Expiration Period}}`: Expiration period for JMS messages.
    - `{{Retention Threshold 4 Alerting}}`: Retention threshold for alerting related to JMS messages.
    - `{{MaxRetries}}`: Maximum number of retries allowed before discarding the message.

- **DataStore / JMS Dependency**
Yes