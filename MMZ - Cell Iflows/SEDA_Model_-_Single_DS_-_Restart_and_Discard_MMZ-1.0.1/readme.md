markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS[(Start 5)]::HTTPS
    DS --> DataStoreConsumer[(Start)]::DataStoreConsumer
    DataStoreConsumer --> ExclusiveGateway_9098
    HTTPS --> ExclusiveGateway_9098
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12
    ExclusiveGateway_9098 -- Discard --> Discaded
    ExclusiveGateway_12 -- Step1 --> Set_Headers_Step1
    ExclusiveGateway_12 -- Step 2 --> Set_Headers_Step2
    ExclusiveGateway_12 -- Step 3 --> Set_Headers_Step3
    ExclusiveGateway_12 -- Unknown --> Custom_Status_Unknown
    Set_Headers_Step1 --> Step1
    Set_Headers_Step2 --> Step2
    Set_Headers_Step3 --> Step3
    Step1 --> Step2_DBstorage
    Step2 --> Step3_DBstorage
    Step3 --> Custom_Status_Step3
    Step2_DBstorage --> Custom_Status_Step2
    Step3_DBstorage --> Custom_Status_Step3
    Custom_Status_Step1 --> End
    Custom_Status_Step2 --> End
    Custom_Status_Step3 --> End
    Custom_Status_Unknown --> End
    Discaded --> Log_Discarded_Message
    Log_Discarded_Message --> Discarded_MaxRetries
    Discarded_MaxRetries --> End
    Step1 --> Step2_DBstorage
    DataStoreConsumer[(Start)]:::DataStoreConsumer
    HTTPS[(Start 5)]:::HTTPS
    classDef adapter fill:#f9f,stroke:#333,stroke-width:2px
    class Postman,DS,DataStoreConsumer,HTTPS adapter
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  The iFlow processes messages using a SEDA (Staged Event-Driven Architecture) router. It receives messages either via HTTPS or from a Data Store, processes them through multiple steps, stores data in a Data Store and includes exception handling with logging. Messages exceeding a maximum number of retries are discarded.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - Sender
    - DS - DataStoreConsumer - Sender

- **Key steps**
 1. Receive message via HTTPS or DataStore.
 2. Determine processing path based on the "Step" header.
 3. Execute Step 1, Step 2, or Step 3, each involving header enrichment and possibly throwing exceptions.
 4. Store data in a Data Store in each step with DBstorage.
 5. Log exceptions asynchronously.
 6. If the message fails repeatedly, discard it after exceeding the maximum retry count.

- **Message transformation**
    - Enrichment activities are used to add headers like `SAP_Sender`, `SAP_Receiver`, `SAP_MessageType`, and `Step`.
    - Custom statuses are added to the message processing log (MPL) at different stages.
    - Message payload may be wrapped in `<Envelope><MessageB64>...</MessageB64></Envelope>`.

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 3 - Maximum number of retries before discarding the message.
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - JMS queue name for SEDA.
    - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
    - `Retry Interval`: 15 - Interval between retry attempts.
    - `Number of Concurrent Processes`: 1 - Number of concurrent processes allowed.
    - `Data Store Name`: SEDA_MODEL_MMZ - Name of the Data Store.
    - `RoleName`: ESBMessaging.send - Role required for sending messages.
    - `Exponential Backoff`: 1 - Flag to enable exponential backoff.
    - `Expiration Period`: 7 - Expiration period for stored messages.
    - `Lock Timeout`: 10 - Timeout for file locking.
    - `Maximum Retry Interval`: 1440 - Maximum interval between retry attempts.
    - `Poll Interval`: 10 - Interval to poll the datastore.

- **DataStore / JMS Dependency**
  Yes

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
    - Log_Exception_Async.groovy - Groovy_Logging_Scripts
    - Log_Discarded_Message.groovy - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
  Not Found