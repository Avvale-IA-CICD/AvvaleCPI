**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    style StartEvent_12079784 fill:#f9f,stroke:#333,stroke-width:2px
    style StartEvent_2 fill:#f9f,stroke:#333,stroke-width:2px
    style EndEvent_2 fill:#f9f,stroke:#333,stroke-width:2px
    style EndEvent_9104 fill:#f9f,stroke:#333,stroke-width:2px

    StartEvent_12079784[Start HTTPS]:::start --> CallActivity_9054[Set Headers]
    StartEvent_2[Start DataStoreConsumer]:::start --> ExclusiveGateway_9098[Reprocess?]

    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12[Step?]
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
    CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
    CallActivity_12079775 --> EndEvent_9104[Discarded MaxRetries]:::end

    ExclusiveGateway_12 -- Step1 --> CallActivity_12079790[Set Headers]
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079792[Set Headers]
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079794[Set Headers]
    ExclusiveGateway_12 -- Unknown --> CallActivity_20[Custom Status]
    CallActivity_20 --> EndEvent_2[End]:::end

    CallActivity_9054 --> CallActivity_12079777[Step1 DataStore]
    CallActivity_12079777 --> CallActivity_9062[Custom Status]
    CallActivity_9062 --> EndEvent_9052[End]

    CallActivity_12079790 --> CallActivity_15[Step 1]
    CallActivity_12079792 --> CallActivity_18[Step 2]
    CallActivity_12079794 --> CallActivity_21[Step 3]

    CallActivity_15 --> CallActivity_12079780[Step2 DataStore]
    CallActivity_18 --> CallActivity_12079781[Step3 DataStore]
    CallActivity_21 --> CallActivity_9074[Custom Status]
    CallActivity_9074 --> EndEvent_2:::end

    CallActivity_12079780 --> CallActivity_9071[Custom Status]
    CallActivity_12079781 --> CallActivity_9077[Custom Status]

    CallActivity_9071 --> EndEvent_2:::end
    CallActivity_9077 --> EndEvent_2:::end

    classDef start fill:#f9f,stroke:#333,stroke-width:2px;
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model for processing messages asynchronously using a Data Store. It includes steps for message processing, error handling, and discarding messages that exceed the maximum retry attempts. The iFlow starts either by a HTTPS call or by consuming a message from a Data Store.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS (Sender)
    - DS - DataStoreConsumer (Sender)

- **Key steps**
    1.  Receive message via HTTPS or DataStore Consumer.
    2.  Determine if the message should be reprocessed based on retry attempts.
    3.  Route the message to different processing steps (Step 1, Step 2, Step 3) based on the `Step` header.
    4.  Each step prepares the message and calls a local integration process.
    5.  If an exception occurs in any step, log the exception and set a custom status.
    6.  If the maximum retry attempts are exceeded, log and discard the message.

- **Message transformation**
    - Enricher components are used to set headers and custom status messages.
    - Groovy scripts are used for logging and potentially throwing exceptions.
    - Content modifiers are used to prepare messages for subsequent steps.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required to access the HTTPS endpoint.
    - `Maximum Retry Interval`: Maximum interval for retrying DataStore consumption.
    - `Exponential Backoff`: Flag to enable exponential backoff for DataStore retries.
    - `Data Store Name`: Name of the Data Store used for persistence.
    - `Poll Interval`: Interval for polling the Data Store.
    - `Retry Interval`: Interval for retrying DataStore consumption.
    - `Lock Timeout`: Timeout for file lock in DataStore.
    - `Retention Threshold 4 Alerting`: Retention threshold for alerting in DataStore.
    - `Expiration Period`: Expiration period for messages in DataStore.
    - `MaxRetries`: Maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts/Log_Discarded_Message.groovy
    - Groovy_Logging_Scripts/Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
Not Found