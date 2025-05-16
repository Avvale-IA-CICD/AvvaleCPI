**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS[Start Event HTTPS]
    DS --> DataStoreConsumer[Start Event DataStore]
    HTTPS --> SetHeaders1
    DataStoreConsumer --> ExclusiveGatewayReprocess
    ExclusiveGatewayReprocess -- Yes --> ExclusiveGatewayStep
    ExclusiveGatewayReprocess -- Discard --> Discarded
    Discarded --> LogDiscardedMessage
    LogDiscardedMessage --> EndDiscardedMaxRetries
    ExclusiveGatewayStep -- Step1 --> SetHeaders2
    ExclusiveGatewayStep -- Step2 --> SetHeaders3
    ExclusiveGatewayStep -- Step 3 --> SetHeaders4
    ExclusiveGatewayStep -- Unknown --> CustomStatusUnknown
    SetHeaders2 --> Step1
    SetHeaders3 --> Step2
    SetHeaders4 --> Step3
    Step1 --> DataStorePut1
    Step2 --> DataStorePut2
    Step3 --> DataStorePut3
    DataStorePut1 --> CustomStatus1
    DataStorePut2 --> CustomStatus2
    DataStorePut3 --> CustomStatus3
    CustomStatus1 --> EndEvent
    CustomStatus2 --> EndEvent
    CustomStatus3 --> EndEvent
    CustomStatusUnknown --> EndEvent
    subgraph Step1
    Step1Start --> PrepareStep2
    PrepareStep2 --> Step1End
    Step1Start:::start --> PrepareStep2:::process
    PrepareStep2:::process --> Step1End:::end
    end
    subgraph Step2
    Step2Start --> PrepareStep3
    PrepareStep3 --> Step2End
    Step2Start:::start --> PrepareStep3:::process
    PrepareStep3:::process --> Step2End:::end
    end
    subgraph Step3
    Step3Start --> TestThrowException
    TestThrowException --> Step3End
    Step3Start:::start --> TestThrowException:::process
    TestThrowException:::process --> Step3End:::end
    end
    style Step1Start fill:#ccf,stroke:#333,stroke-width:2px
    style Step2Start fill:#ccf,stroke:#333,stroke-width:2px
    style Step3Start fill:#ccf,stroke:#333,stroke-width:2px
    style Step1End fill:#ccf,stroke:#333,stroke-width:2px
    style Step2End fill:#ccf,stroke:#333,stroke-width:2px
    style Step3End fill:#ccf,stroke:#333,stroke-width:2px
    style ExclusiveGatewayStep fill:#f9f,stroke:#333,stroke-width:2px
    style ExclusiveGatewayReprocess fill:#f9f,stroke:#333,stroke-width:2px
    style DataStorePut1 fill:#ffc,stroke:#333,stroke-width:2px
    style DataStorePut2 fill:#ffc,stroke:#333,stroke-width:2px
    style DataStorePut3 fill:#ffc,stroke:#333,stroke-width:2px
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model using a single Data Store. It includes steps for processing messages, handling exceptions, and discarding messages after a certain number of retries. The flow starts with either an HTTPS call or a DataStore Consumer, processes the message through several steps, and logs exceptions asynchronously.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - DS - DataStoreConsumer - EndpointSender

- **Key steps**
    1.  Receive message via HTTPS or DataStore Consumer.
    2.  Set initial headers.
    3.  Store the message in DataStore (Step1).
    4.  Route the message based on the 'Step' header.
    5.  Process message through Step 1, Step 2, and Step 3 local integration processes.
    6.  Store the message in DataStore for each step (Step2, Step3).
    7.  Set custom status for each step completion.
    8.  Handle exceptions in each step and log them asynchronously.
    9.  Discard messages exceeding the maximum retry count.

- **Message transformation**
    - Enricher activities are used to set headers and custom status.
    - Groovy scripts are used for logging and exception handling.
    - Content modifiers are used to prepare messages for subsequent steps.

- **Externalized parameters list and their descriptions**
    - RoleName: Role required to access the HTTPS endpoint.
    - Maximum Retry Interval: Maximum interval between retries for DataStore Consumer.
    - Exponential Backoff: Flag to enable exponential backoff for DataStore Consumer retries.
    - Data Store Name: Name of the Data Store used for message persistence.
    - Poll Interval: Interval for polling the Data Store.
    - Retry Interval: Interval between retries for DataStore Consumer.
    - Lock Timeout: Timeout for file lock in DataStore Consumer.
    - Retention Threshold 4 Alerting: Retention threshold for alerting in DataStore.
    - Expiration Period: Expiration period for messages in DataStore.
    - MaxRetries: Maximum number of retries before discarding a message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Discarded_Message.groovy - Groovy_Logging_Scripts
    - Log_Exception_Async.groovy - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found