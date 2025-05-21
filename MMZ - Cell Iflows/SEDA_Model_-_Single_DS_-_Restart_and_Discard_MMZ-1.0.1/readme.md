**iFlowId**: SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman -->|HTTPS| DummyStart
    DS -->|DataStoreConsumer| SEDA_Router
    subgraph DummyStart [Dummy Start]
        DummyStart --> SetHeaders
        SetHeaders --> Step1_DS_Put
        Step1_DS_Put --> CustomStatus0
        CustomStatus0 --> End_DummyStart
    end

    subgraph SEDA_Router [SEDA Router]
        SEDA_Router --> ReprocessDecision{Reprocess?}
        ReprocessDecision -- Yes --> StepDecision{Step?}
        ReprocessDecision -- Discard --> DiscardedStatus
        StepDecision -- Step1 --> SetHeaders1
        StepDecision -- Step2 --> SetHeaders2
        StepDecision -- Step 3 --> SetHeaders3
        StepDecision -- Unknown --> CustomStatusUnknown
        SetHeaders1 --> Step1
        SetHeaders2 --> Step2
        SetHeaders3 --> Step3
        DiscardedStatus --> LogDiscardedMessage
        LogDiscardedMessage --> End_Discarded

        subgraph Step1 [Step 1]
            Step1 --> PrepareStep2
        end

        subgraph Step2 [Step 2]
            Step2 --> PrepareStep3
        end

         subgraph Step3 [Step 3]
            Step3 --> TestThrowException
        end

        Step1 --> Step2_DS_Put
        Step2 --> Step3_DS_Put
        Step2_DS_Put --> CustomStatus1
        Step3_DS_Put --> CustomStatus2
        CustomStatus1 --> End_SEDA
        CustomStatus2 --> End_SEDA
        CustomStatusUnknown --> End_SEDA
        PrepareStep2 --> Step2_DS_Put

        PrepareStep3 --> Step3_DS_Put

        LogDiscardedMessage --> End_Discarded
        End_Discarded --> End_SEDA
        End_SEDA --> End_SEDA
    end
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_DS_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
-   **Brief description of the iFlow**
    This iFlow simulates a message processing scenario where messages are retrieved from a Data Store, processed through a series of steps, and stored back into the Data Store. The flow includes error handling, logging, and a retry mechanism with a discard option for messages exceeding the maximum retry attempts. It also includes REST endpoint to start the flow.
-   **Involved systems with Adapters Type and Endpoint Type**
    -   Postman: HTTPS (Sender)
    -   DS: DataStoreConsumer (Sender)
-   **Key steps**
    1.  Receive message via HTTPS endpoint.
    2.  Store message data into Data Store (Step1).
    3.  Retrieve message from Data Store using SEDA Router.
    4.  Process message through steps 1, 2, and 3.
    5.  Store message in Data Store (Step 2, Step 3).
    6.  Log custom status for each step.
    7.  Discard the message if maximum retries are exceeded.
-   **Message transformation**
    -   Content Enrichers are used in Dummy Start Process to set message headers (SAP_Sender, SAP_Receiver, SAP_MessageType, Step).
    -   Content Enrichers are used in Step 0, Step 1, Step 2 and Step 3 subprocesses to add custom status.
    -   The 'Prepare Step' enrichers create the 'Step' header and wraps content in an envelope containing a base64 encoded message.
-   **Externalized parameters list, configured values and their descriptions**
    -   MaxRetries: 3 - Maximum number of retries before discarding the message.
    -   SEDA_MAIN_QUEUE: SEDA_MODEL_MMZ - SEDA queue name.
    -   Retention Threshold 4 Alerting: 1 - Retention threshold for alerting.
    -   Retry Interval: 15 - Interval between retry attempts in seconds.
    -   Number of Concurrent Processes: 1 - Number of concurrent processes.
    -   Data Store Name: SEDA_MODEL_MMZ - Name of the Data Store.
    -   RoleName: ESBMessaging.send - Role required to access the HTTPS endpoint.
    -   Exponential Backoff: 1 - Exponential backoff setting (likely enabled).
    -   Expiration Period: 7 - Expiration period for the Data Store entries in days.
    -   Lock Timeout: 10 - Lock timeout in seconds.
    -   Maximum Retry Interval: 1440 - Maximum retry interval in seconds.
    -   Poll Interval: 10 - Poll interval in seconds.
-   **DataStore / JMS Dependency**
    Yes
-   **Cloud Connector Dependency**
    Not Found
-   **Common Scripts Dependency**
    -   Log_Discarded_Message.groovy - scriptBundleId: Groovy_Logging_Scripts
    -   Log_Exception_Async.groovy - scriptBundleId: Groovy_Logging_Scripts
-   **ProcessDirect ComponentType Dependency**
    Not Found