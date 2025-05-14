**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue for asynchronous message processing. It demonstrates error handling, message retries, and discarding messages after reaching a maximum retry limit or encountering an unknown routing condition. The flow processes messages in three steps and logs exceptions.

- **Involved systems**
    - SQUEUE (Sender Queue)
    - RQUEUE (Receiver Queue)
    - Postman (for triggering the flow via HTTPS)

- **Used Adapters**
    - JMS (Java Message Service)
    - HTTPS

- **Key steps**
    1. Receive a message from SQUEUE via JMS adapter. The message can also be trigger via HTTPS from postman
    2. Determine the 'Step' property.
    3. Route the message based on the 'Step' property to respective processes: Step 1, Step 2, or Step 3.
    4. Each step updates headers and prepares the message for the next step and resends to the JMS Queue (RQUEUE) to be processed by the next step.
    5. If the maximum retry count is exceeded or if the step is unknown, the message is discarded.
    6. Exceptions are logged in dedicated subprocesses.

- **Message transformation**
    - The iFlow uses Enricher components to set headers (e.g., SAP_Sender, SAP_Receiver, SAP_MessageType) and properties to keep track of message status and processing stages. The flow also use "Custom Status" components to create the SAP_MessageProcessingLogCustomStatus.
    - Groovy scripts are used to log discarded messages and exceptions.
    - Step 2 and Step 1 processes use a content modifier to prepare the message for next step.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the JMS queue used for asynchronous message processing.
    - `Number of Concurrent Processes`: Number of concurrent processes for the JMS dispatcher.
    - `Maximum Retry Interval`: Maximum time interval between retry attempts (likely in seconds).
    - `Retry Interval`: Initial time interval between retry attempts (likely in seconds).
    - `Use Dead Letter Queue`: Flag to indicate whether to use a dead letter queue for failed messages (value should be 1).
    - `ExponentialBackoff`: Flag to indicate whether to use exponential backoff for retry attempts (value should be 1).
    - `Retention Threshold 4 Alerting`: Threshold for retention alerting (likely in days).
    - `Expiration Period`: Time period after which messages expire (likely in days).
    - `MaxRetries`: Maximum number of retries before a message is discarded.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    subgraph Dummy Start
        Postman[Postman\n(HTTPS)] --> Start6((Start\n(HTTPS)))
        Start6 --> SetHeaders0[Set Headers\n(Enricher)]
        SetHeaders0 --> SaveInitialMsg[Save Initial Msg\n(Content Modifier)]
        SaveInitialMsg --> CustomStatus0[Custom Status\n(Enricher)]
        CustomStatus0 --> End5((End))
    end

    subgraph SEDA Router
        SQUEUE[SQUEUE\n(JMS)] --> Start((Start))
        Start --> Reprocess{Reprocess?\n(Exclusive Gateway)}
        Reprocess -- Yes --> StepCheck{Step?\n(Exclusive Gateway)}
        Reprocess -- Discard --> DiscardedMessageCS[Custom Status\n(Discarded)]
        DiscardedMessageCS --> LogDiscardedMessage[Log Discarded Message]
        LogDiscardedMessage --> DiscardedMaxRetries((Discarded MaxRetries))

        StepCheck -- Step1 --> SetHeaders1[Set Headers\n(Enricher)]
        StepCheck -- Step2 --> SetHeaders2[Set Headers\n(Enricher)]
        StepCheck -- Step3 --> SetHeaders3[Set Headers\n(Enricher)]
        StepCheck -- Unknown --> CustomStatusUnknown[Custom Status\n(Unknown)]
        CustomStatusUnknown --> LogDiscardedMessageUnknown[Log Discarded Message]
        LogDiscardedMessageUnknown --> DiscardedUnknown((Discarded Unknown))
        
        SetHeaders1 --> Step1(Step 1)
        SetHeaders2 --> Step2(Step 2)
        SetHeaders3 --> Step3(Step 3)

        Step1 --> NextStep1[Next Step\n(Content Modifier)]
        Step2 --> NextStep2[Next Step\n(Content Modifier)]
        Step3 --> CustomStatus3[Custom Status\n(Step 3 Completed)]

        NextStep1 --> CustomStatus1[Custom Status\n(Step 1 Completed)]
        NextStep2 --> CustomStatus2[Custom Status\n(Step 2 Completed)]

        CustomStatus1 --> End((End))
        CustomStatus2 --> End((End))
        CustomStatus3 --> End((End))
    end
    
    subgraph Step1 Process
        Step1 --> Start2((Start))
        Start2 --> PrepareStep2[Prepare Step 2\n(Content Modifier)]
        PrepareStep2 --> End2((End))
    end

    subgraph Step2 Process
        Step2 --> Start3((Start))
        Start3 --> PrepareStep3[Prepare Step 3\n(Content Modifier)]
        PrepareStep3 --> End3((End))
    end

     subgraph Step3 Process
        Step3 --> Start4((Start))
        Start4 --> TestThrowException[Test Throw Exception\n(Groovy Script)]
        TestThrowException --> End4((End))
    end
    Start6 -.-> Start
```