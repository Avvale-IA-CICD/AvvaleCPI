**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman[Postman]
    DS[DataStore]
    DummyStart[Dummy Start]
    SEDARouter[SEDA Router]
    Step1[Step 1]
    Step2[Step 2]
    Step3[Step 3]
    Discarded[Discarded MaxRetries]
    End[End]

    Postman -- HTTPS Adapter --> DummyStart
    DS -- DataStoreConsumer Adapter --> SEDARouter

    SEDARouter --  --> Step1
    SEDARouter --  --> Step2
    SEDARouter --  --> Step3
    SEDARouter --  --> Discarded
    SEDARouter --  --> End

    Step1 --> SEDARouter
    Step2 --> SEDARouter
    Step3 --> SEDARouter
    Discarded --> SEDARouter
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model with a single DataStore. It retrieves messages from a DataStore or via HTTPS, processes them through multiple steps, and includes logic for retry, discarding messages after a maximum number of retries and exception handling.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - Sender
    - DS - DataStoreConsumer - Sender

- **Key steps**
    1.  Receive message from DataStore or HTTPS endpoint.
    2.  Determine if the message needs to be reprocessed based on retry attempts. If maximum retries are exceeded, discard the message.
    3.  Route the message to different processing steps based on the `Step` header.
    4.  Process message through Step 1, Step 2, and Step 3. Each step prepares the message and stores it in the DataStore.
    5.  Set custom status in each step to track message processing.
    6.  Log exceptions for each step.
    7.  After processing all steps, the iFlow ends.

- **Message transformation**
    - The iFlow uses "Enricher" components to set headers and custom statuses.
    - Headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, and `Step` are set based on the current processing stage.
    - Custom statuses (`SAP_MessageProcessingLogCustomStatus`) are created to reflect the completion of each step.
    - Groovy scripts are used for logging messages and exceptions

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 3 - Maximum number of retries before discarding a message.
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - Name of the JMS queue.
    - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
    - `Retry Interval`: 15 - Interval between retry attempts in seconds.
    - `Number of Concurrent Processes`: 1 - Number of concurrent processes.
    - `Data Store Name`: SEDA_MODEL_MMZ - Name of the DataStore.
    - `RoleName`: ESBMessaging.send - Role required for sending messages.
    - `Exponential Backoff`: 1 - Whether to use exponential backoff for retries (1 for yes, 0 for no).
    - `Expiration Period`: 7 - Expiration period for messages in days.
    - `Lock Timeout`: 10 - Timeout for locking the DataStore entry.
    - `Maximum Retry Interval`: 1440 - Maximum retry interval in minutes.
    - `Poll Interval`: 10 - Interval for polling the DataStore in seconds.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Discarded_Message.groovy - Groovy_Logging_Scripts
    - Log_Exception_Async.groovy - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found