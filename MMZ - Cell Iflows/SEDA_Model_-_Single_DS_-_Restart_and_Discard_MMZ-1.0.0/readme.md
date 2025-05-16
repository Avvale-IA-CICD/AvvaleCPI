**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    style StartEvent_HTTPS fill:#FFB6C1,stroke:#333,stroke-width:2px
    style StartEvent_DataStore fill:#FFB6C1,stroke:#333,stroke-width:2px
    style EndEvent fill:#FFB6C1,stroke:#333,stroke-width:2px

    StartEvent_HTTPS[HTTPS Start Event]:::start --> ReprocessQuestion{Reprocess?}
    StartEvent_DataStore[DataStore Start Event]:::start --> ReprocessQuestion

    ReprocessQuestion -- Yes --> StepQuestion{Step?}
    ReprocessQuestion -- Discard --> DiscardedStatus[Discarded Status]

    DiscardedStatus --> LogDiscardedMessage[Log Discarded Message - Groovy]
    LogDiscardedMessage --> EndEvent_Discarded[Discarded MaxRetries]:::end

    StepQuestion -- Step1 --> SetHeaders_Step1[Set Headers - Step1]
    StepQuestion -- Step2 --> SetHeaders_Step2[Set Headers - Step2]
    StepQuestion -- Step3 --> SetHeaders_Step3[Set Headers - Step3]
    StepQuestion -- Unknown --> CustomStatus_Unknown[Custom Status - Unknown]

    SetHeaders_Step1 --> Step1[Step 1]
    SetHeaders_Step2 --> Step2[Step 2]
    SetHeaders_Step3 --> Step3[Step 3]
    CustomStatus_Unknown --> EndEvent[End Event]:::end

    Step1 --> DBStorage_Step2[Step2 - DBstorage]
    Step2 --> DBStorage_Step3[Step3 - DBstorage]
    Step3 --> CustomStatus_Step3[Custom Status - Step3]

    DBStorage_Step2 --> CustomStatus_Step1[Custom Status - Step1]
    DBStorage_Step3 --> CustomStatus_Step2[Custom Status - Step2]
    CustomStatus_Step3 --> EndEvent

    CustomStatus_Step1 --> EndEvent
    CustomStatus_Step2 --> EndEvent
    classDef start fill:#FFB6C1,stroke:#333,stroke-width:2px
    classDef end fill:#FFB6C1,stroke:#333,stroke-width:2px
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow processes messages retrieved from a Data Store, routes them based on a 'Step' header, and either processes them through a series of steps or discards them if the maximum retry count is exceeded. Exception handling is included for each step, logging exceptions asynchronously.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - DS - DataStoreConsumer - EndpointSender

- **Key steps**
    1.  Receive message from HTTPS endpoint or DataStore.
    2.  Determine if the message should be reprocessed based on retry count. If max retries are exceeded, discard the message.
    3.  Route the message based on the 'Step' header to Step 1, Step 2, or Step 3. If the step is unknown, set a custom status.
    4.  Each step (Step 1, Step 2, Step 3) prepares the message, calls a local integration process, stores the message in the Data Store, and sets a custom status.
    5.  Log any exceptions that occur during the steps asynchronously.

- **Message transformation**
    - Enricher components are used to set headers (e.g., SAP_Sender, SAP_Receiver, SAP_MessageType, Step) and custom statuses (SAP_MessageProcessingLogCustomStatus).
    - Groovy scripts are used to log discarded messages and exceptions.
    - Prepare Step 2 and Step 3 enrichers add a header 'Step' and wrap content in an envelope.

- **Externalized parameters list and their descriptions**
    - RoleName: Role required to access the HTTPS endpoint.
    - Maximum Retry Interval: Maximum interval for retrying DataStore consumption.
    - Exponential Backoff: Flag to enable exponential backoff for DataStore retries.
    - Data Store Name: Name of the Data Store used for persistence.
    - Poll Interval: Interval for polling the Data Store.
    - Retry Interval: Interval for retrying DataStore consumption.
    - Lock Timeout: Timeout for file locking in the DataStore.
    - Retention Threshold 4 Alerting: Retention threshold for alerting in DB storage.
    - Expiration Period: Expiration period for messages in DB storage.
    - MaxRetries: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts - Log_Discarded_Message.groovy
    - Groovy_Logging_Scripts - Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
Not Found