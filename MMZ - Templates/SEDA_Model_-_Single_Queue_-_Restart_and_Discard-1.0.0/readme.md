**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Mermaid Diagram**
- **Visual representation of the flow**

```mermaid
graph LR
    Postman --> HTTPS_Start[HTTPS Start]
    SQUEUE --> JMS_Start[JMS Start]
    JMS_Start --> Reprocess{Reprocess?}
    Reprocess -- Yes --> StepCheck{Step?}
    Reprocess -- Discard --> DiscardMaxRetriesStatus[Discard Max Retries Status]
    DiscardMaxRetriesStatus --> LogDiscardedMessageMaxRetries[Log Discarded Message Max Retries]
    LogDiscardedMessageMaxRetries --> EndDiscardMaxRetries[End Discard MaxRetries]
    StepCheck -- Step1 --> SetHeadersStep1[Set Headers Step 1]
    SetHeadersStep1 --> Step1[Step 1]
    Step1 --> JMS_Step1[JMS Step1]
    JMS_Step1 --> CustomStatusStep1[Custom Status Step1]
    CustomStatusStep1 --> End
    StepCheck -- Step2 --> SetHeadersStep2[Set Headers Step 2]
    SetHeadersStep2 --> Step2[Step 2]
    Step2 --> JMS_Step2[JMS Step2]
    JMS_Step2 --> CustomStatusStep2[Custom Status Step2]
    CustomStatusStep2 --> End
    StepCheck -- Step3 --> SetHeadersStep3[Set Headers Step 3]
    SetHeadersStep3 --> Step3[Step 3]
    Step3 --> JMS_Step3[JMS Step3]
    JMS_Step3 --> CustomStatusStep3[Custom Status Step3]
    CustomStatusStep3 --> End
    StepCheck -- Unknown --> DiscardUnknown[Discard Unknown]
    DiscardUnknown --> LogDiscardedMessageUnknown[Log Discarded Message Unknown]
    LogDiscardedMessageUnknown --> EndDiscardedUnknown[End Discarded Unknown]
    subgraph Step1
        StartStep1[Start Step 1] --> PrepareStep2[Prepare Step 2]
        PrepareStep2 --> EndStep1[End Step 1]
        PrepareStep2 -.-> LogAsyncExceptionStep1((Log Async Exception))
        StartError1[Error Start 1] --> CustomStatusError1[Custom Status Error 1]
        CustomStatusError1 --> LogAsyncExceptionStep1
        LogAsyncExceptionStep1 --> EndError1[Error End 1]
    end
    subgraph Step2
        StartStep2[Start Step 2] --> PrepareStep3[Prepare Step 3]
        PrepareStep3 --> EndStep2[End Step 2]
        PrepareStep3 -.-> LogAsyncExceptionStep2((Log Async Exception))
        StartError2[Error Start 2] --> CustomStatusError2[Custom Status Error 2]
        CustomStatusError2 --> LogAsyncExceptionStep2
        LogAsyncExceptionStep2 --> EndError2[Error End 2]
    end
    subgraph Step3
        StartStep3[Start Step 3] --> TestThrowException[Test Throw Exception]
        TestThrowException --> EndStep3[End Step 3]
        TestThrowException -.-> LogAsyncExceptionStep3((Log Async Exception))
        StartError3[Error Start 3] --> CustomStatusError3[Custom Status Error 3]
        CustomStatusError3 --> LogAsyncExceptionStep3
        LogAsyncExceptionStep3 --> EndError3[Error End 3]
    end
    HTTPS_Start --> SaveInitialMsg[Save Initial Msg]
    SaveInitialMsg --> SetHeadersDummyStart[Set Headers Dummy Start]
    SetHeadersDummyStart --> EndDummyStart[End Dummy Start]
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue. It receives messages, processes them in multiple steps (Step 1, Step 2, Step 3), and handles exceptions by logging them and potentially discarding messages after a certain number of retries. The iFlow also logs custom status messages at various points in the flow.

- **Involved systems**
    - SQUEUE (Source Queue)
    - RQUEUE (Receiver Queue)
    - Postman (HTTP Sender)

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
    1.  Receive message from SQUEUE via JMS adapter.
    2.  Determine if the message needs to be reprocessed or discarded based on the number of retries.
    3.  Route the message to Step 1, Step 2, or Step 3 based on the "Step" property.
    4.  Each step (Step 1, Step 2, Step 3) prepares the message, executes its specific logic, and sets a custom status.
    5.  If an exception occurs in any step, log the exception and set a custom status. Discard the message if it fails repeatedly, log discarded messages and register corresponding custom statuses.

- **Message transformation**
    - Enricher: Used to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType, SAP_MessageProcessingLogCustomStatus) and properties (Step) at various stages of the flow.
    - Groovy Scripts: Used to log exceptions and discarded messages with a custom script.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: Name of the JMS queue used for asynchronous processing.
    - Number of Concurrent Processes: Number of concurrent processes used by the JMS adapter.
    - Maximum Retry Interval: Maximum retry interval for the JMS adapter.
    - Expiration Period: Expiration period for JMS messages.
    - Retention Threshold 4 Alerting: Retention threshold for JMS alerting.
    - Retry Interval: Retry interval for the JMS adapter.
    - MaxRetries: Maximum retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

- **Cloud Connector Dependency**
Not Found