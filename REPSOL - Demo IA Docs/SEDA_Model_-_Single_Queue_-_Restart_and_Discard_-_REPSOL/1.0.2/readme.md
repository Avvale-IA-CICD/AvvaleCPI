**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_-_REPSOL - **iFlowVersion**: 1.0.2

**Mermaid Diagram**
```mermaid
graph LR
    SQUEUE[SQUEUE] --> JMS_DISPATCHER(StartEvent_2: Start)
    StartEvent_2 --> Reprocess{Reprocess?}
    Reprocess -- Yes --> StepCheck{Step?}
    Reprocess -- Discard --> DiscardMaxRetriesStatus[CallActivity_9101: Discaded]
    DiscardMaxRetriesStatus --> LogDiscardedMaxRetries[CallActivity_12079775: Log Discarded Message]
    LogDiscardedMaxRetries --> DiscardMaxRetries(EndEvent_9104: Discarded MaxRetries)

    StepCheck -- Step1 --> SetHeadersStep1[CallActivity_12079818: Set Headers]
    StepCheck -- Step2 --> SetHeadersStep2[CallActivity_12079821: Set Headers]
    StepCheck -- Step3 --> SetHeadersStep3[CallActivity_12079824: Set Headers]
    StepCheck -- Unknown --> DiscardUnknownStepStatus[CallActivity_20: Custom Status]
    DiscardUnknownStepStatus --> LogDiscardedUnknown[CallActivity_12079827: Log Discarded Message]
    LogDiscardedUnknown --> DiscardUnknown(EndEvent_12079826: Discarded Unknown)

    SetHeadersStep1 --> Step1[CallActivity_15: Step 1]
    Step1 --> JMSSendStep1(ServiceTask_56: Next Step)
    JMSSendStep1 --> CustomStatusStep1[CallActivity_9071: Custom Status]
    CustomStatusStep1 --> EndEvent_2(EndEvent_2: End)
    JMSSendStep1 --> RQUEUE[RQUEUE]

    SetHeadersStep2 --> Step2[CallActivity_18: Step 2]
    Step2 --> JMSSendStep2(ServiceTask_9069: Next Step)
    JMSSendStep2 --> CustomStatusStep2[CallActivity_9077: Custom Status]
    CustomStatusStep2 --> EndEvent_2
    JMSSendStep2 --> RQUEUE

    SetHeadersStep3 --> Step3[CallActivity_21: Step 3]
    Step3 --> CustomStatusStep3[CallActivity_9074: Custom Status]
    CustomStatusStep3 --> EndEvent_2

    CustomStatusStep3 --> EndEvent_2
```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_-_REPSOL-1.0.2.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow implements a SEDA (Staged Event-Driven Architecture) model using a single JMS queue. It demonstrates message processing across multiple steps (Step 1, Step 2, Step 3) with restart and discard mechanisms based on the number of retries. It includes exception handling and logging.

- **Involved systems with Adapters Type and Endpoint Type**
  - SQUEUE: JMS (EndpointSender)
  - RQUEUE: JMS (EndpointRecevier)
  - Postman: HTTPS (EndpointSender)

- **Key steps**
  1.  Receive a message via JMS from SQUEUE.
  2.  Check retry count and discard based on 'MaxRetries' threshold.
  3.  Route the message to different processing steps (Step 1, Step 2, Step 3) based on the `Step` property. Each step sets headers and custom statuses for logging.
  4.  Each step can optionally throw an exception, handled by a dedicated exception subprocess.
  5. Send messages to RQUEUE after each processing step.
  6.  If the `Step` property is not recognized, the message is discarded.

- **Message transformation**
  - Enrichers are used to set headers (SAP_Sender, SAP_Receiver, SAP_MessageType) and custom statuses (SAP_MessageProcessingLogCustomStatus).
  - Content modifiers are used within each step to prepare messages.
  - Groovy scripts are used for logging exceptions and discarding messages.

- **Externalized parameters list, configured values and their descriptions**
  - `MaxRetries`: 10 - Maximum number of retries before discarding a message.
  - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - Name of the main JMS queue.
  - `Expiration Period`: 7 - Expiration period for messages.
  - `Maximum Retry Interval`: 1440 - Maximum interval between retries (in minutes).
  - `Retention Threshold 4 Alerting`: 1 - Retention threshold for alerting.
  - `Retry Interval`: 15 - Interval between retries (in minutes).
  - `Number of Concurrent Processes`: 1 - Number of concurrent processes.

- **DataStore / JMS Dependency**
  Yes

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  - Groovy_Logging_Scripts: Log_Discarded_Message.groovy
  - Groovy_Logging_Scripts: Log_Exception_Async.groovy

- **ProcessDirect ComponentType Dependency**
  Not Found