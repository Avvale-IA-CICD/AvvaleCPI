markdown
**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman[Postman]
    DataStore[DataStore]
    StartEvent_12079784((Start Event))
    StartEvent_2((Start Event))
    ExclusiveGateway_9098{Reprocess?}
    ExclusiveGateway_12{Step?}
    CallActivity_12079790[Set Headers Step1]
    CallActivity_15[Step 1]
    CallActivity_12079780[Step2]
    CallActivity_9071[Custom Status Step1Completed]
    CallActivity_12079792[Set Headers Step2]
    CallActivity_18[Step 2]
    CallActivity_12079781[Step3]
    CallActivity_9077[Custom Status Step2Completed]
    CallActivity_12079794[Set Headers Step3]
    CallActivity_21[Step 3]
    CallActivity_9074[Custom Status Step3Completed]
    CallActivity_20[Custom Status UnknownStep]
    CallActivity_9101[Custom Status Discaded]
    EndEvent_2((End Event))
    EndEvent_9104((End Event Discarded))

    Postman -- HTTPS --> StartEvent_12079784
    DataStore -- DataStoreConsumer --> StartEvent_2
    StartEvent_2 --> ExclusiveGateway_9098
    ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12
    ExclusiveGateway_9098 -- Discard --> CallActivity_9101
    CallActivity_9101 --> EndEvent_9104
    ExclusiveGateway_12 -- Step1 --> CallActivity_12079790
    ExclusiveGateway_12 -- Step 2 --> CallActivity_12079792
    ExclusiveGateway_12 -- Step 3 --> CallActivity_12079794
    ExclusiveGateway_12 -- Unknown --> CallActivity_20
    CallActivity_12079790 --> CallActivity_15
    CallActivity_12079792 --> CallActivity_18
    CallActivity_12079794 --> CallActivity_21
    CallActivity_15 --> CallActivity_12079780
    CallActivity_18 --> CallActivity_12079781
    CallActivity_21 --> CallActivity_9074
    CallActivity_12079780 --> CallActivity_9071
    CallActivity_12079781 --> CallActivity_9077
    CallActivity_9071 --> EndEvent_2
    CallActivity_9077 --> EndEvent_2
    CallActivity_9074 --> EndEvent_2
    CallActivity_20 --> EndEvent_2
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow processes messages through a series of steps (Step 1, Step 2, Step 3) and uses a Data Store for persistence and restart capabilities. It handles exceptions, logs errors, and discards messages exceeding the maximum retry limit. The iFlow is triggered either by an HTTPS call or by the Data Store Consumer.

- **Involved systems with Adapters Type and Endpoint Type**
    - Postman - HTTPS - Sender
    - DS - DataStoreConsumer - Sender

- **Key steps**
    1. Receive message from HTTPS endpoint or DataStore Consumer.
    2. Set initial headers and store the message in the DataStore.
    3. Route the message to subsequent steps (Step 1, Step 2, Step 3) based on the 'Step' header.
    4. Each step prepares the message, calls a sub-process and updates the message processing log.
    5. Store messages in the DataStore after each step
    6. Handle exceptions and log errors using a dedicated sub-process.
    7. Discard messages exceeding the 'MaxRetries' limit and log the discarded message.
    8. Complete message processing and set the final status.

- **Message transformation**
    - The iFlow uses Enrichers to set headers and custom status messages at various points.
    - Step 2 and Step 1 and Step 3 enricher use constant values to prepare sub process calls
    - Groovy scripts are used for exception logging and discarded message logging.

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 3 - Maximum number of retries before discarding a message.
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - JMS Queue Name (not used)
    - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting (days).
    - `Retry Interval`: 15 - Retry interval in minutes for DataStore consumption.
    - `Number of Concurrent Processes`: 1 - Number of concurrent processes (not used).
    - `Data Store Name`: SEDA_MODEL_MMZ - Name of the DataStore used for message persistence.
    - `RoleName`: ESBMessaging.send - Role required for sending messages via HTTPS.
    - `Exponential Backoff`: 1 - Enable exponential backoff for DataStore consumption retries.
    - `Expiration Period`: 7 - Expiration period in days for DataStore entries.
    - `Lock Timeout`: 10 - Lock timeout (unit unspecified) for DataStore consumption.
    - `Maximum Retry Interval`: 1440 - Maximum retry interval in minutes for DataStore consumption.
    - `Poll Interval`: 10 - Poll interval in seconds for DataStore consumption.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
    - Log_Exception_Async.groovy - scriptBundleId: Groovy_Logging_Scripts
    - Log_Discarded_Message.groovy - scriptBundleId: Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
Not Found