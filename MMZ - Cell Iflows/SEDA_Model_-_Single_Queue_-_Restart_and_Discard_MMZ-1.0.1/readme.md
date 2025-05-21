markdown
**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
```mermaid
graph LR
    Postman --> HTTPS[Start Event HTTPS]
    HTTPS --> SaveInitialMsg[Save Initial Message]
    SaveInitialMsg --> JMS_STEP0(RQUEUE) JMS Adapter
    JMS_STEP0 --> StartEvent[Start Event JMS]
    StartEvent --> ReprocessCheck{Reprocess?}
    ReprocessCheck -- Discard --> DiscardedEnricher[Discarded]
    DiscardedEnricher --> LogDiscardedMessage1[Log Discarded Message]
    LogDiscardedMessage1 --> DiscardedMaxRetries[End Event Discarded Max Retries]
    ReprocessCheck -- Yes --> StepCheck{Step?}
    StepCheck -- Step1 --> SetHeaders1[Set Headers Step1]
    SetHeaders1 --> Step1Call[Call Step 1]
    Step1Call --> JMS_STEP1(RQUEUE) JMS Adapter
    StepCheck -- Step 2 --> SetHeaders2[Set Headers Step2]
    SetHeaders2 --> Step2Call[Call Step 2]
    Step2Call --> JMS_STEP2(RQUEUE) JMS Adapter
    StepCheck -- Step 3 --> SetHeaders3[Set Headers Step3]
    SetHeaders3 --> Step3Call[Call Step 3]
    Step3Call --> JMS_STEP3(RQUEUE) JMS Adapter
    StepCheck -- Unknown --> DiscardedEnricher2[Discarded Status Unknown]
    DiscardedEnricher2 --> LogDiscardedMessage2[Log Discarded Message]
    LogDiscardedMessage2 --> DiscardedUnknown[End Event Discarded Unknown]
    JMS_STEP1 --> Step1Status[Set Custom Status Step1]
    Step1Status --> EndEvent[End Event]
    JMS_STEP2 --> Step2Status[Set Custom Status Step2]
    Step2Status --> EndEvent
    JMS_STEP3 --> Step3Status[Set Custom Status Step3]
    Step3Status --> EndEvent

```
**BPMN Diagram**

![BPMN Diagram](./SEDA_Model_-_Single_Queue_-_Restart_and_Discard_MMZ-1.0.1.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
  This iFlow demonstrates a SEDA (Staged Event-Driven Architecture) model with a single queue. It processes messages through multiple steps, handling exceptions, retries, and message discarding based on a maximum retry count.  The flow begins with an HTTPS call, then utilizes JMS queues to move messages between integration processes representing different steps. Exception subprocesses log any errors which occur asynchronously.

- **Involved systems with Adapters Type and Endpoint Type**
  - Postman - HTTPS - EndpointSender
  - SQUEUE - JMS - EndpointSender
  - RQUEUE - JMS - EndpointRecevier

- **Key steps**
  1.  An HTTPS request initiates the flow.
  2.  The initial message is saved and headers are set.
  3.  The message is routed through multiple integration processes (Step 1, Step 2, Step 3) via JMS queues.  Each step prepares the message for the following step, setting headers and custom statuses.
  4.  If the message fails during processing at any step, an exception subprocess logs the exception asynchronously.
  5.  The SEDA Router checks if the number of retries has exceeded the configured maximum.  If so, the message is discarded and logged. If the receiver is unknown, it will also be discarded.

- **Message transformation**
  - Enricher components are used within Dummy Start, Step 1, Step 2, Step 3 and SEDA Router processes to set headers and properties. Prepare Step 2 and Prepare Step 3 processes use enrichers to set the "Step" property and create XML envelopes.

- **Externalized parameters list, configured values and their descriptions**
  - `MaxRetries`: 10 - Maximum number of retries before discarding a message.
  - `SEDA_MAIN_QUEUE`: SEDA_MODEL_MMZ - The name of the main JMS queue used for message processing.
  - `Expiration Period`: 7 - The expiration period for messages in the queue (likely in days, but units are not specified).
  - `Maximum Retry Interval`: 1440 - The maximum interval between retry attempts (likely in minutes, but units are not specified).
  - `Retention Threshold 4 Alerting`: 1 - Threshold for retention alerting (units are not specified).
  - `Retry Interval`: 15 - The interval between retry attempts (likely in minutes, but units are not specified).
  - `Number of Concurrent Processes`: 1 - The number of concurrent processes for the JMS adapter.

- **DataStore / JMS Dependency**
  Yes

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  - Log_Discarded_Message.groovy - Groovy_Logging_Scripts
  - Log_Exception_Async.groovy - Groovy_Logging_Scripts

- **ProcessDirect ComponentType Dependency**
  Not Found