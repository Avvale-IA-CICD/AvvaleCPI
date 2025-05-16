**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
flowchart LR
    Postman[Postman]
    DS[DS]

    Postman -- HTTPS --> StartEvent_12079784((Start Event))
    DS -- DataStoreConsumer --> StartEvent_2((Start))

    StartEvent_12079784 --> SetHeaders0[Set Headers]
    StartEvent_2 --> Reprocess{Reprocess?}
    Reprocess -- Yes --> StepGateway{Step?}
    Reprocess -- Discard --> Discarded[Discaded]
    Discarded --> LogDiscarded[Log Discarded Message]
    LogDiscarded --> DiscardedMaxRetries((Discarded MaxRetries))

    StepGateway -- Step1 --> SetHeaders1[Set Headers]
    StepGateway -- Step2 --> SetHeaders2[Set Headers]
    StepGateway -- Step 3 --> SetHeaders3[Set Headers]
    StepGateway -- Unknown --> UnknownStep[Custom Status]
    UnknownStep --> End((End))

    SetHeaders0 --> Step1_DS[Step1]
    Step1_DS --> CustomStatus0[Custom Status]
    CustomStatus0 --> End

    SetHeaders1 --> Step1[Step 1]
    Step1 --> Step2_DS[Step2]
    Step2_DS --> CustomStatus1[Custom Status]
    CustomStatus1 --> End

    SetHeaders2 --> Step2[Step 2]
    Step2 --> Step3_DS[Step3]
    Step3_DS --> CustomStatus2[Custom Status]
    CustomStatus2 --> End

    SetHeaders3 --> Step3[Step 3]
    Step3 --> CustomStatus3[Custom Status]
    CustomStatus3 --> End

    subgraph Step1Process [Step 1]
        StartEvent_37((Start 2)) --> PrepareStep2[Prepare Step 2]
        PrepareStep2 --> EndEvent_38((End 2))
    end

    subgraph Step2Process [Step 2]
        StartEvent_41((Start 3)) --> PrepareStep3[Prepare Step 3]
        PrepareStep3 --> EndEvent_42((End 3))
    end

    subgraph Step3Process [Step 3]
        StartEvent_45((Start 4)) --> TestThrowException[Test Throw Exception]
        TestThrowException --> EndEvent_46((End 4))
    end
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow processes messages retrieved from a DataStore, routes them through a series of steps (Step 1, Step 2, Step 3), and handles exceptions that may occur during processing. It includes retry logic and discards messages that exceed the maximum retry attempts.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - DS - DataStoreConsumer - EndpointSender

- **Key steps**
    1.  Receive message via HTTPS from Postman or from DataStore.
    2.  Determine if the message should be reprocessed based on retry attempts.
    3.  Route the message to Step 1, Step 2, or Step 3 based on the `Step` header.
    4.  Execute the corresponding steps (Step 1, Step 2, Step 3) which involve preparing the step and potentially throwing an exception.
    5.  Store the message in the DataStore after each step.
    6.  If the message exceeds the maximum retry attempts, discard it.
    7.  Log exceptions that occur during the process.

- **Message transformation**
    - Enricher activities are used to set headers and custom status messages at various points in the iFlow.
    - Groovy scripts are used for logging and potentially throwing exceptions.
    - Prepare Step activities use Enrichers to set the Step header and wrap the message content.

- **Externalized parameters list and their descriptions**
    - `RoleName`: Role required to access the HTTPS endpoint.
    - `Maximum Retry Interval`: Maximum interval for retrying DataStore operations.
    - `Exponential Backoff`: Flag to enable exponential backoff for DataStore retries.
    - `Data Store Name`: Name of the DataStore used for message persistence.
    - `Poll Interval`: Interval for polling the DataStore.
    - `Retry Interval`: Interval for retrying DataStore operations.
    - `Lock Timeout`: Timeout for file locking in the DataStore.
    - `Retention Threshold 4 Alerting`: Threshold for alerting on data retention.
    - `Expiration Period`: Period after which data expires.
    - `MaxRetries`: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Discarded_Message.groovy - Groovy_Logging_Scripts
    - Log_Exception_Async.groovy - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found