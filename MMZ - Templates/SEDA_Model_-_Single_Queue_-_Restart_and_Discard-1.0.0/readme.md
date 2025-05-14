**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue for asynchronous processing. It receives messages, routes them through different processing steps (simulated by direct calls to local integration processes), and handles potential exceptions by logging them and discarding messages that exceed the maximum retry count or have an unknown step.

- **Involved systems**
    - SQUEUE (Sender Queue)
    - RQUEUE (Receiver Queue)
    - Postman (HTTP Client for triggering the flow)

- **Used Adapters**
    - JMS (Java Message Service)
    - HTTPS

- **Key steps**
    i.  Receive message from SQUEUE via JMS adapter.
    ii. Route message based on the `Step` property (Step1, Step2, Step3 or Unknown).
    iii. Each step (Step1, Step2, Step3) calls a dedicated local integration process that prepares and sends the message to the next step.
    iv. If a step fails, an exception subprocess logs the error.
    v. If the message has been retried more than the maximum allowed times (`MaxRetries`), or if the step is unknown, the message is discarded, and an appropriate log is written.
    vi. The final message is sent to RQUEUE via JMS adapter.
    vii. Postman sends message via HTTPs adapter that triggers the flow.

- **Message transformation**
    - The iFlow enriches the message with headers (like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`) and custom statuses for logging purposes.
    - Content modifiers ("Enrichers") are used to create message bodies with static content.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: The name of the JMS queue used for message exchange between components.
    - `Retention Threshold 4 Alerting`: Threshold for message retention alerting (JMS adapter setting)
    - `Expiration Period`: Message expiration period (JMS adapter setting)
    - `Number of Concurrent Processes`: Number of concurrent processes for the JMS receiver adapter.
    - `Maximum Retry Interval`: Maximum retry interval for the JMS receiver adapter.
    - `Retry Interval`: Retry interval for the JMS receiver adapter.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman] --> B(HTTPS Start Event)
    B --> C{Set Headers - Step0}
    C --> D[Save Initial Msg - JMS_STEP0]
    D --> E{Custom Status - Step0Completed}
    E --> F(JMS - SQUEUE)
    F --> G{Start - SEDA Router}
    G --> H{Reprocess?}
    H -- Discard --> I[Custom Status - DiscardedMaxRetries]
    I --> J(Log Discarded Message)
    J --> K[Discarded MaxRetries - End Event]
    H -- Yes --> L{Step?}
    L -- Step1 --> M{Set Headers - Step1}
    M --> N[Step 1 - Call Process_36]
    N --> O(JMS - RQUEUE)
    O --> P{Custom Status - Step1Completed}
    P --> Q[End - SEDA Router]
    L -- Step 2 --> R{Set Headers - Step2}
    R --> S[Step 2 - Call Process_40]
    S --> T(JMS - RQUEUE)
    T --> U{Custom Status - Step2Completed}
    U --> Q
     L -- Step 3 --> V{Set Headers - Step3}
    V --> W[Step 3 - Call Process_44]
    W --> X(JMS - RQUEUE)
    X --> Y{Custom Status - Step3Completed}
    Y --> Q
    L -- Unknown --> Z[Custom Status - DiscardedUnknownStep]
    Z --> AA(Log Discarded Message)
    AA --> BB[Discarded Unknown - End Event]

    subgraph Process_36
        S37[Start 2] --> C48[Prepare Step 2]
        C48 --> E38[End 2]
    end

    subgraph Process_40
        S41[Start 3] --> C54[Prepare Step 3]
        C54 --> E42[End 3]
    end

     subgraph Process_44
        S45[Start 4] --> C12079771[Test Throw Exception]
        C12079771 --> E46[End 4]
    end
```
