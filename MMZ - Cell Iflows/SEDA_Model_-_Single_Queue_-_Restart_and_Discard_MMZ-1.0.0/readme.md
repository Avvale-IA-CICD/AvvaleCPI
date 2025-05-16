**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    style StartEvent_2 fill:#FFB6C1,stroke:#333,stroke-width:2px
    style StartEvent_12079806 fill:#FFB6C1,stroke:#333,stroke-width:2px
    style EndEvent_2 fill:#FFB6C1,stroke:#333,stroke-width:2px
    style EndEvent_9104 fill:#FFB6C1,stroke:#333,stroke-width:2px
    style EndEvent_12079826 fill:#FFB6C1,stroke:#333,stroke-width:2px

    Participant_1[SQUEUE JMS Sender] --> StartEvent_2(Start JMS)
    Participant_12079805[Postman HTTPS Sender] --> StartEvent_12079806(Start HTTPS)
    StartEvent_12079806 --> CallActivity_9054[Set Headers]
    CallActivity_9054 --> ServiceTask_9058[Save Initial Msg]
    ServiceTask_9058 --> CallActivity_9062[Custom Status]
    CallActivity_9062 --> EndEvent_9052(End)
    ServiceTask_9058 --> Participant_59[RQUEUE JMS Receiver]

    StartEvent_2 --> ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12{Step?}
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
    CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
    CallActivity_12079775 --> EndEvent_9104(Discarded MaxRetries)

    ExclusiveGateway_12 -- Step1 --> CallActivity_12079818[Set Headers]
    CallActivity_12079818 --> CallActivity_15[Step 1]
    CallActivity_15 --> ServiceTask_56[Next Step]
    ServiceTask_56 --> CallActivity_9071[Custom Status]
    ServiceTask_56 --> Participant_59
    CallActivity_9071 --> EndEvent_2(End)

    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079821[Set Headers]
    CallActivity_12079821 --> CallActivity_18[Step 2]
    CallActivity_18 --> ServiceTask_9069[Next Step]
    ServiceTask_9069 --> CallActivity_9077[Custom Status]
    ServiceTask_9069 --> Participant_59
    CallActivity_9077 --> EndEvent_2(End)

    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079824[Set Headers]
    CallActivity_12079824 --> CallActivity_21[Step 3]
    CallActivity_21 --> CallActivity_9074[Custom Status]
    CallActivity_21 --> Participant_59
    CallActivity_9074 --> EndEvent_2(End)

    ExclusiveGateway_12 -- Unknown --> CallActivity_20[Custom Status]
    CallActivity_20 --> CallActivity_12079827[Log Discarded Message]
    CallActivity_12079827 --> EndEvent_12079826(Discarded Unknown)
```

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) pattern with a single queue. It receives messages, processes them through multiple steps, and handles exceptions. It also includes retry and discard mechanisms for failed messages.

-   **Involved systems with Adapters Type and Endpoint Type**
    -   SQUEUE: JMS Adapter, EndpointSender
    -   RQUEUE: JMS Adapter, EndpointRecevier
    -   Postman: HTTPS Adapter, EndpointSender

-   **Key steps**
    1.  Receive message via JMS adapter from SQUEUE.
    2.  Route the message based on the `Step` property.
    3.  Execute Step 1, Step 2, or Step 3 based on the routing.
    4.  Each step prepares the message for the next step and sets headers.
    5.  If the `Step` property is unknown, discard the message.
    6.  If the message exceeds the maximum retries, discard the message.
    7.  Log exceptions during asynchronous processing.
    8.  Send message via JMS adapter to RQUEUE.

-   **Message transformation**
    -   Each step enriches the message with specific headers and properties to prepare it for the next step.
    -   The content of the message is modified in each step.
    -   Custom statuses are added to the message processing log.

-   **Externalized parameters list and their descriptions**
    -   `SEDA_MAIN_QUEUE`: The name of the JMS queue used for message exchange.
    -   `Number of Concurrent Processes`: The number of concurrent processes for the JMS receiver adapter.
    -   `Maximum Retry Interval`: The maximum retry interval for the JMS receiver adapter.
    -   `Retry Interval`: The retry interval for the JMS receiver adapter.
    -   `Retention Threshold 4 Alerting`: Retention threshold for alerting.
    -   `Expiration Period`: Expiration period for messages.
    -   `MaxRetries`: Maximum number of retries before discarding the message.

-   **DataStore / JMS Dependency**
    Yes

-   **Cloud Connector Dependency**
    Not Found

-   **Common Scripts Dependency**
    -   Groovy\_Logging\_Scripts/Log\_Discarded\_Message.groovy
    -   Groovy\_Logging\_Scripts/Log\_Exception\_Async.groovy

-   **ProcessDirect ComponentType Dependency**
    Not Found