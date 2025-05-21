**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS_Adapter --> DummyStart
    DummyStart --> JMS_Adapter --> SEDA_Router

    subgraph SEDA_Router
    Start --> Reprocess_Check
    Reprocess_Check -- Yes --> Step_Check
    Reprocess_Check -- Discard --> Discarded_Status
    Step_Check -- Step1 --> SetHeaders_Step1
    Step_Check -- Step 2 --> SetHeaders_Step2
    Step_Check -- Step 3 --> SetHeaders_Step3
    Step_Check -- Unknown --> Discarded_Unknown
    SetHeaders_Step1 --> Step1
    SetHeaders_Step2 --> Step2
    SetHeaders_Step3 --> Step3
    Step1 --> JMS_Adapter --> Step1_Completed
    Step2 --> JMS_Adapter --> Step2_Completed
    Step3 --> JMS_Adapter --> Step3_Completed
    Step1_Completed --> End
    Step2_Completed --> End
    Step3_Completed --> End
    Discarded_Unknown --> Log_Discarded_Unknown
    Discarded_Status --> Log_Discarded_MaxRetries
    Log_Discarded_Unknown --> Discard_Unknown
    Log_Discarded_MaxRetries --> Discard_MaxRetries
    end

    Discard_MaxRetries --> End
    Discard_Unknown --> End

    subgraph Step1
    Step1Start --> Prepare_Step2
    Prepare_Step2 --> Step1End
    end

     subgraph Step2
    Step2Start --> Prepare_Step3
    Prepare_Step3 --> Step2End
    end

     subgraph Step3
    Step3Start --> Test_Throw_Exception
    Test_Throw_Exception --> Step3End
    end
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow simulates a multi-step asynchronous process using JMS queues and SEDA (Staged Event-Driven Architecture) for routing. It includes error handling with exception subprocesses and message discarding based on retry counts. The process is triggered by an HTTPS call, and messages are passed through different steps.

- **Involved systems with Adapters Type and Endpoint Type**
    - SQUEUE: JMS (EndpointSender)
    - Postman: HTTPS (EndpointSender)
    - RQUEUE: JMS (EndpointRecevier)

- **Key steps**
    1.  Receive a message via HTTPS.
    2.  Save the initial message and set headers.
    3.  The "SEDA Router" process determines which step to execute based on the `Step` property.
    4.  Each step (Step 1, Step 2, Step 3) prepares a message and then calls a subsequent process.
    5.  After each step custom statuses are set and the next queue "Next Step" is defined through a Service Task.
    6.  If a step fails, an exception subprocess logs the error.
    7.  If the message exceeds the maximum retry count, it's discarded. Otherwise it continues in the SEDA Router.
    8.  A script logs discarded messages.

- **Message transformation**
    - Enricher components are used to set headers and properties at various stages.
    - Constant values and expressions are used to manipulate message content.
    - Scripts are used to log exception and discarded messages.

- **Externalized parameters list, configured values and their descriptions**
    - `MaxRetries`: 10 (Maximum number of retries before discarding a message)
    - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ (Main queue used for SEDA routing)
    - `Expiration Period`: 7 (Expiration period for messages)
    - `Maximum Retry Interval`: 1440 (Maximum retry interval in minutes)
    - `Retention Threshold 4 Alerting`: 1 (Retention threshold for alerting)
    - `Retry Interval`: 15 (Retry interval in minutes)
    - `Number of Concurrent Processes`: 1 (Number of concurrent processes for JMS)

- **DataStore / JMS Dependency**
    Yes

- **Cloud Connector Dependency**
    Not Found

- **Common Scripts Dependency**
    - `Log_Exception_Async.groovy` (scriptBundleId: Groovy_Logging_Scripts)
    - `Log_Discarded_Message.groovy` (scriptBundleId: Groovy_Logging_Scripts)

- **ProcessDirect ComponentType Dependency**
    Not Found