**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS[Start]
    HTTPS -- HTTPS Adapter --> Start5(Start 5)
    Start5 --> SetHeaders1
    SetHeaders1 --> Step1DS[()]
    Step1DS --> CustomStatus1
    CustomStatus1 --> End5(End 5)
    End5 --> Router{Reprocess?}
    Router -- Yes --> StepRouter{Step?}
    Router -- Discard --> DiscardedStatus
    DiscardedStatus --> LogDiscarded
    LogDiscarded --> DiscardedMaxRetries(Discarded MaxRetries)
    StepRouter -- Step1 --> SetHeaders2
    StepRouter -- Step2 --> SetHeaders3
    StepRouter -- Step3 --> SetHeaders4
    StepRouter -- Unknown --> UnknownStep
    SetHeaders2 --> Step1(Step 1)
    SetHeaders3 --> Step2(Step 2)
    SetHeaders4 --> Step3(Step 3)
    UnknownStep --> End(End)
    Step1 --> Step2DS[()]
    Step2DS --> CustomStatus2
    CustomStatus2 --> End
    Step2 --> Step3DS[()]
    Step3DS --> CustomStatus3
    CustomStatus3 --> End
    Step3 --> Step4DS[()]
    Step4DS --> CustomStatus4
    CustomStatus4 --> End
    DataStore -- DataStoreConsumer Adapter --> Start(Start)
    Start --> Router
```

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model using a single DataStore. It includes restart and discard mechanisms for handling messages. The flow involves multiple steps, including setting headers, storing messages in a DataStore, and logging exceptions. It also includes retry logic and discarding of messages exceeding the maximum retry attempts.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - EndpointSender
    - DS - DataStoreConsumer - EndpointSender

- **Key steps**
    1.  Receive message via HTTPS adapter from Postman.
    2.  Set initial headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step).
    3.  Store the message in the DataStore (Step1).
    4.  Route the message based on the 'Step' header to different processing steps (Step1, Step2, Step3).
    5.  Each step sets specific headers, calls a local integration process, and stores the message in the DataStore.
    6.  If the message fails and exceeds the maximum retry attempts, it is discarded and logged.
    7.  Exceptions are caught and logged asynchronously.
    8.  Custom statuses are set at various points in the flow for monitoring.

- **Message transformation**
    - Enricher components are used to set headers and custom statuses.
    - Groovy scripts are used for logging exceptions and discarded messages.
    - Content modifiers prepare messages for subsequent steps.

- **Externalized parameters list and their descriptions**
    - RoleName: Role required to access the HTTPS endpoint.
    - Maximum Retry Interval: Maximum interval between retries for DataStore consumer.
    - Exponential Backoff: Flag to enable exponential backoff for DataStore consumer retries.
    - Data Store Name: Name of the DataStore used for message persistence.
    - Poll Interval: Interval for polling the DataStore.
    - Retry Interval: Interval between retries for DataStore consumer.
    - Lock Timeout: Timeout for file lock in DataStore consumer.
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