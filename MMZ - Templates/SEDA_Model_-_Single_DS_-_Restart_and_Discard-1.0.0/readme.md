**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
```mermaid
graph LR
    subgraph DummyStart [Dummy Start]
        StartEvent_12079784((Start)) --> CallActivity_9054[Set Headers]
        CallActivity_9054 --> CallActivity_12079777[Step1]
        CallActivity_12079777 --> CallActivity_9062[Custom Status]
        CallActivity_9062 --> EndEvent_9052((End))
    end
    
    subgraph SEDARouter [SEDA Router]
        StartEvent_2((Start)) --> ExclusiveGateway_9098{Reprocess?}
        ExclusiveGateway_9098 -- Yes --> ExclusiveGateway_12{Step?}
        ExclusiveGateway_9098 -- Discard --> CallActivity_9101[Discaded]
        CallActivity_9101 --> CallActivity_12079775[Log Discarded Message]
        CallActivity_12079775 --> EndEvent_9104((Discarded MaxRetries))
        ExclusiveGateway_12 -- Step1 --> CallActivity_12079790[Set Headers]
        CallActivity_12079790 --> CallActivity_15[Step 1]
        CallActivity_15 --> CallActivity_12079780[Step2]
        CallActivity_12079780 --> CallActivity_9071[Custom Status]
        CallActivity_9071 --> EndEvent_2((End))
        ExclusiveGateway_12 -- Step 2 --> CallActivity_12079792[Set Headers]
        CallActivity_12079792 --> CallActivity_18[Step 2]
        CallActivity_18 --> CallActivity_12079781[Step3]
        CallActivity_12079781 --> CallActivity_9077[Custom Status]
        CallActivity_9077 --> EndEvent_2
        ExclusiveGateway_12 -- Step 3 --> CallActivity_12079794[Set Headers]
        CallActivity_12079794 --> CallActivity_21[Step 3]
        CallActivity_21 --> CallActivity_9074[Custom Status]
        CallActivity_9074 --> EndEvent_2
        ExclusiveGateway_12 -- Unknown --> CallActivity_20[Custom Status]
        CallActivity_20 --> EndEvent_2
    end

    subgraph Step1Process [Step 1]
        StartEvent_37((Start)) --> CallActivity_48[Prepare Step 2]
        CallActivity_48 --> EndEvent_38((End))
    end

    subgraph Step2Process [Step 2]
       StartEvent_41((Start)) --> CallActivity_54[Prepare Step 3]
        CallActivity_54 --> EndEvent_42((End))
    end
    
    subgraph Step3Process [Step 3]
       StartEvent_45((Start)) --> CallActivity_12079788[Test Throw Exception]
        CallActivity_12079788 --> EndEvent_46((End))
    end

    Postman[Postman] --> DummyStart
    DS[DS] --> SEDARouter
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow simulates an asynchronous process where messages are picked up from a Data Store, processed through multiple steps, and stored back into the Data Store. It demonstrates error handling, message reprocessing, and discarding messages after a certain number of retries. It leverages SEDA router to handle the process asynchronously.

- **Involved systems**
    - Postman
    - DS (Data Store)

- **Used Adapters**
    - HTTPS
    - DataStoreConsumer

- **Key steps**
    1. Receive message from Postman via HTTPS or from Data Store via DataStoreConsumer.
    2. Determine if message needs to be reprocessed (based on retry logic).
    3. Route message based on "Step" header to appropriate sub-process (Step1, Step2, Step3, or Unknown).
    4. Each step sub-process prepares the message, calls a subsequent sub-process, and updates the Data Store.
    5. If MaxRetries has been reached, message will be discarded and message will be logged.

- **Message transformation**
    - Enricher components are used to set headers and custom message processing log statuses at various stages.
    - Groovy scripts are used for logging discarded messages and exceptions.
    - Message content is transformed to prepare for subsequent steps, particularly in Step 1, Step 2 and Step 3.

- **Externalized parameters list and their descriptions**
    - RoleName: Role required to access the HTTPS endpoint.
    - Maximum Retry Interval: Maximum interval between retries for the DataStoreConsumer.
    - Exponential Backoff: Flag to enable exponential backoff for DataStoreConsumer retries.
    - Data Store Name: Name of the Data Store used for persistence.
    - Poll Interval: Interval for polling the Data Store.
    - Retry Interval: Interval between retries for the DataStoreConsumer.
    - Lock Timeout: Timeout for file lock in DataStoreConsumer.
    - Retention Threshold 4 Alerting: Retention threshold for alerting in datastore.
    - Expiration Period: Expiration Period for message in DataStore.
    - MaxRetries: The maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found