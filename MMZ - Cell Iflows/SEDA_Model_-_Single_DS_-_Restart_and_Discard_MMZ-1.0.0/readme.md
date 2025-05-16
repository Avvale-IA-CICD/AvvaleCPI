markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman[Postman]
    DS[DS]
    StartHTTPS[Start HTTPS]
    StartDataStore[Start DataStore]
    CheckRetries{Reprocess?}
    Route{Step?}
    Step1SetHeaders[Set Headers - Step1]
    Step2SetHeaders[Set Headers - Step2]
    Step3SetHeaders[Set Headers - Step3]
    Step1Process[Step 1]
    Step2Process[Step 2]
    Step3Process[Step 3]
    Step1DataStore[Step1]
    Step2DataStore[Step2]
    Step3DataStore[Step3]
    Step1CustomStatus[Custom Status - Step1]
    Step2CustomStatus[Custom Status - Step2]
    Step3CustomStatus[Custom Status - Step3]
    UnknownStep[Custom Status - Unknown]
    DiscardMessage[Discaded]
    LogDiscarded[Log Discarded Message]
    EndDiscarded[Discarded MaxRetries]
    End[End]

    Postman -- HTTPS --> StartHTTPS
    DS -- DataStoreConsumer --> StartDataStore
    StartDataStore --> CheckRetries
    StartHTTPS --> CheckRetries
    CheckRetries -- Yes --> Route
    CheckRetries -- Discard --> DiscardMessage
    DiscardMessage --> LogDiscarded
    LogDiscarded --> EndDiscarded
    Route -- Step1 --> Step1SetHeaders
    Route -- Step 2 --> Step2SetHeaders
    Route -- Step 3 --> Step3SetHeaders
    Route -- Unknown --> UnknownStep
    Step1SetHeaders --> Step1Process
    Step2SetHeaders --> Step2Process
    Step3SetHeaders --> Step3Process
    Step1Process --> Step1DataStore
    Step2Process --> Step2DataStore
    Step3Process --> Step3DataStore
    Step1DataStore --> Step1CustomStatus
    Step2DataStore --> Step2CustomStatus
    Step3DataStore --> Step3CustomStatus
    Step1CustomStatus --> End
    Step2CustomStatus --> End
    Step3CustomStatus --> End
    UnknownStep --> End
    EndDiscarded --> End
```
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow processes messages retrieved from a Data Store, routes them based on the 'Step' header, and stores them back in the Data Store after each step. It includes error handling and a mechanism to discard messages that exceed a maximum retry count.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - Sender
    - DS - DataStoreConsumer - Sender

- **Key steps**
 1. Receives message from HTTPS endpoint or DataStore.
 2. Checks if the message should be reprocessed based on retry count. If retry count exceeds the maximum, the message is discarded.
 3. Routes the message based on the value of the 'Step' header (Step1, Step2, Step3, or Unknown).
 4. For each step (Step1, Step2, Step3), the iFlow calls a local integration process to prepare the message, then stores the message in the Data Store.
 5. Sets a custom status in the message processing log after each step.
 6. If the step is unknown, sets a custom status and ends the flow.
 7. Logs any exceptions that occur during the process.

- **Message transformation**
    - The iFlow uses Enrichers to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step) with constant values.
    - The iFlow uses Enrichers to set custom status messages in the message processing log using expressions.
    - Step 2 and Step 1 local integration processes use Enrichers to prepare the message with a constant body and header.

- **Externalized parameters list and their descriptions**
    - RoleName: User role for HTTPS sender authentication.
    - Maximum Retry Interval: Maximum interval for retries.
    - Exponential Backoff: Flag to enable exponential backoff for retries.
    - Data Store Name: Name of the Data Store.
    - Poll Interval: Interval for polling the Data Store.
    - Retry Interval: Interval for retries.
    - Lock Timeout: Timeout for file lock.
    - Retention Threshold 4 Alerting: Retention threshold for alerting.
    - Expiration Period: Expiration period for data in the Data Store.
    - MaxRetries: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found