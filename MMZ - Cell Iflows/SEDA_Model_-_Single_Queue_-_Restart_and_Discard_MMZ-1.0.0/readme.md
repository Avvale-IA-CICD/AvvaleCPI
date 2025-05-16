**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS_Sender((Start Dummy Start))
    SQUEUE --> JMS_Sender((Start SEDA Router))
    JMS_Sender --> ExclusiveGateway_Reprocess{Reprocess?}
    ExclusiveGateway_Reprocess -- Yes --> ExclusiveGateway_Step{Step?}
    ExclusiveGateway_Reprocess -- Discard --> Discarded_MaxRetries_Enricher[Discaded]
    Discarded_MaxRetries_Enricher --> Log_Discarded_Message((Log Discarded Message))
    Log_Discarded_Message --> DiscardedMaxRetries((Discarded MaxRetries))
    ExclusiveGateway_Step -- Step1 --> SetHeaders_Step1[Set Headers]
    SetHeaders_Step1 --> Step1((Step 1))
    Step1 --> JMS_STEP1[Next Step]
    JMS_STEP1 --> CustomStatus_Step1[Custom Status]
    CustomStatus_Step1 --> EndSEDA((End))
    ExclusiveGateway_Step -- Step 2 --> SetHeaders_Step2[Set Headers]
    SetHeaders_Step2 --> Step2((Step 2))
    Step2 --> JMS_STEP2[Next Step]
    JMS_STEP2 --> CustomStatus_Step2[Custom Status]
    CustomStatus_Step2 --> EndSEDA
    ExclusiveGateway_Step -- Step 3 --> SetHeaders_Step3[Set Headers]
    SetHeaders_Step3 --> Step3((Step 3))
    Step3 --> CustomStatus_Step3[Custom Status]
    CustomStatus_Step3 --> EndSEDA
    ExclusiveGateway_Step -- Unknown --> Discarded_Unknown_Enricher[Custom Status]
    Discarded_Unknown_Enricher --> Log_Discarded_Message_Unknown((Log Discarded Message))
    Log_Discarded_Message_Unknown --> DiscardedUnknown((Discarded Unknown))
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single queue. It receives messages, processes them in multiple steps, and handles exceptions by logging them and discarding messages that exceed the maximum retry count or have an unknown processing step.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - SQUEUE - JMS - EndpointSender
    - RQUEUE - JMS - EndpointRecevier

- **Key steps**
    1. Receive message via HTTPS or JMS.
    2. Determine the processing step based on a property.
    3. Execute the corresponding processing step (Step 1, Step 2, or Step 3).
    4. Log exceptions and discard messages exceeding retry limits or with unknown steps.
    5. Send message to the next step via JMS.

- **Message transformation**
    - The iFlow uses Enrichers to set headers and properties for message routing and logging.
    - Step 1, Step 2 and Step 3 processes use Enrichers to prepare the message body with a base64 encoded message.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the JMS queue used for message exchange between steps.
    - `Number of Concurrent Processes`: Number of concurrent processes for the JMS adapter.
    - `Maximum Retry Interval`: Maximum retry interval for the JMS adapter.
    - `Retry Interval`: Retry interval for the JMS adapter.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting in the JMS adapter.
    - `Expiration Period`: Expiration period for messages in the JMS adapter.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts/Log_Discarded_Message.groovy
    - Groovy_Logging_Scripts/Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
Not Found