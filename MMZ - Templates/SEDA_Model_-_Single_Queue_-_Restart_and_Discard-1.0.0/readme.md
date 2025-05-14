**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using JMS queues. It receives messages, processes them in multiple steps, and handles potential exceptions. The flow includes retry logic and a discard mechanism for messages that exceed the maximum retry limit or encounter unknown processing steps.

- **Involved systems**
    - SQUEUE: Sender JMS Queue
    - RQUEUE: Receiver JMS Queue
    - Postman (for initial HTTPS trigger)

- **Used Adapters**
    - JMS (Sender and Receiver)
    - HTTPS (Receiver)

- **Key steps**
    i.  Receive message via HTTPS (triggered by Postman) and save the initial message to JMS.
    ii. Route message via SEDA router to Step 1, Step 2, and Step 3 based on the Step property.
    iii. Each Step prepares a follow up message and puts it on the Main Queue.
    iv. The SEDA router checks the Step property. If the Step is unknown, the message is discarded.
    v.  If an error occurs in any of the steps, an exception subprocess is triggered, logging the error.
    vi. If the number of retries (SAPJMSRetries header) exceeds the maximum defined retries (MaxRetries parameter), the message is discarded. Messages are put back onto the main queue.

- **Message transformation**
    - Set Headers: Used to set message headers like `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` at various stages of the iFlow.
    - Prepare Step X: Used to set the Step property and create a message body for each step.
    - Groovy Scripts: Used to log exceptions and discarded messages.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: The name of the main JMS queue used for message processing.
    - Retention Threshold 4 Alerting: Threshold for alerting on message retention.
    - Expiration Period: Message expiration period.
    - Number of Concurrent Processes: Number of concurrent processes for the JMS receiver.
    - Maximum Retry Interval: The maximum interval between retries for the JMS receiver.
    - Retry Interval: Interval for retrying message processing for the JMS receiver.
    - MaxRetries: Maximum number of retries before a message is discarded.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    A[Postman] --> B(HTTPS Start Event)
    B --> C{Set Headers (Step0)}
    C --> D(Save Initial Msg to JMS)
    D --> E{Custom Status (Step0Completed)}
    E --> F(End Event)

    F --> G{JMS Start Event}
    G --> H{Reprocess?}
    H -- Yes --> I{Step?}
    H -- Discard --> J{Custom Status (DiscardedMaxRetries)}
    J --> K(Log Discarded Message)
    K --> L(End Event - MaxRetries)

    I -- Step1 --> M{Set Headers (Step1)}
    M --> N(Call Step 1)
    N --> O(Next Step - JMS)
    O --> P{Custom Status (Step1Completed)}
    P --> Q(End Event)

    I -- Step2 --> R{Set Headers (Step2)}
    R --> S(Call Step 2)
    S --> T(Next Step - JMS)
    T --> U{Custom Status (Step2Completed)}
    U --> V(End Event)

    I -- Step 3 --> W{Set Headers (Step3)}
    W --> X(Call Step 3)
    X --> Y{Custom Status (Step3Completed)}
    Y --> Z(End Event)

    I -- Unknown --> AA{Custom Status (DiscardedUnknownStep)}
    AA --> BB(Log Discarded Message)
    BB --> CC(End Event - Discarded Unknown)

    style J fill:#f9f,stroke:#333,stroke-width:2px
    style K fill:#f9f,stroke:#333,stroke-width:2px
    style L fill:#f9f,stroke:#333,stroke-width:2px
    style AA fill:#f9f,stroke:#333,stroke-width:2px
    style BB fill:#f9f,stroke:#333,stroke-width:2px
    style CC fill:#f9f,stroke:#333,stroke-width:2px
```