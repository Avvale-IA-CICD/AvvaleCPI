**iFlowId**: SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion**: 1.0.0
**Mermaid Diagram**```mermaid
graph LR
    SQUEUE -- JMS_DISPATCHER --> Start
    Postman -- HTTPS --> StartDummy
    Start --> Reprocess
    Reprocess -- Discard --> DiscardMaxRetries
    Reprocess -- Yes --> StepDecision
    StepDecision -- Step1 --> SetHeaders1
    StepDecision -- Step2 --> SetHeaders2
    StepDecision -- Step3 --> SetHeaders3
    StepDecision -- Unknown --> DiscardUnknown
    SetHeaders1 --> Step1
    SetHeaders2 --> Step2
    SetHeaders3 --> Step3
    Step1 --> NextStep1
    Step2 --> NextStep2
    Step3 --> NextStep3
    NextStep1 --> CustomStatus1
    NextStep2 --> CustomStatus2
    NextStep3 --> CustomStatus3
    CustomStatus1 --> End
    CustomStatus2 --> End
    CustomStatus3 --> End
    DiscardMaxRetries --> LogDiscardedMaxRetries
    DiscardUnknown --> CustomStatusUnknown
    LogDiscardedMaxRetries --> EndDiscardedMaxRetries
    CustomStatusUnknown --> LogDiscardedUnknown
    LogDiscardedUnknown --> EndDiscardedUnknown
    StartDummy --> SetHeaders0
    SetHeaders0 --> SaveInitialMsg
    SaveInitialMsg --> CustomStatus0
    CustomStatus0 --> EndDummy
    
    subgraph Step1Subprocess
    Start1Exception --> CustomStatusStep1Exception
    CustomStatusStep1Exception --> LogAsyncException1
    LogAsyncException1 --> End1Exception
    end

    subgraph Step2Subprocess
    Start2Exception --> CustomStatusStep2Exception
    CustomStatusStep2Exception --> LogAsyncException2
    LogAsyncException2 --> End2Exception
    end
    
    subgraph Step3Subprocess
    Start3Exception --> CustomStatusStep3Exception
    CustomStatusStep3Exception --> LogAsyncException3
    LogAsyncException3 --> End3Exception
    end

    subgraph DummyStartException
    StartDummyException --> LogAsyncExceptionDummy
    LogAsyncExceptionDummy --> EndDummyException
    end
    
    Step1 -- Exception --> Start1Exception
    Step2 -- Exception --> Start2Exception
    Step3 -- Exception --> Start3Exception
    StartDummy -- Exception --> StartDummyException

    classDef exceptionFill fill:#f9f,stroke:#333,stroke-width:2px;
    class Start1Exception,Start2Exception,Start3Exception,StartDummyException exceptionFill
    classDef endEventFill fill:#ccf,stroke:#333,stroke-width:2px;
    class End endEventFill
    classDef discarded fill:#fcc,stroke:#333,stroke-width:2px;
    class EndDiscardedMaxRetries,EndDiscardedUnknown discarded
    classDef endDummy fill:#cfc,stroke:#333,stroke-width:2px;
    class EndDummy endDummy
    
    
```
**Functional Summary**- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern using a single JMS queue. It receives messages, processes them in multiple steps (Step 1, Step 2, Step 3), and handles potential exceptions during each step. The iFlow also includes retry logic with a maximum retry count and discards messages that exceed the retry limit or encounter an unknown routing destination. It logs async exceptions.

- **Involved systems**
    - SQUEUE
    - RQUEUE
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
    1. Receive message via JMS adapter from SQUEUE.
    2. Determine the next step based on the `Step` property.
    3. If `Step` = Step1:
        - Call "Step 1" integration process.
        - Send message to RQUEUE via JMS adapter.
    4. If `Step` = Step2:
        - Call "Step 2" integration process.
        - Send message to RQUEUE via JMS adapter.
    5. If `Step` = Step3:
        - Call "Step 3" integration process.
        - Send message to RQUEUE via JMS adapter.
    6. If step is unknown, discard message.
    7. Implement retry logic with discard functionality if MaxRetries parameter is reached
    8. An initial Dummy Start process is triggered via HTTPs call from Postman, to start the process

- **Message transformation**
    - The iFlow enriches messages with custom status and headers in multiple steps.
    - Prepare Step 2: Sets the Step property to Step2 and adds a Step2Message in base64 encoded format in the message body.
    - Prepare Step 3: Sets the Step property to Step3 and adds a Step3Message in base64 encoded format in the message body.
    - Several "Set Headers" steps enrich the message with `SAP_Sender`, `SAP_Receiver`, and `SAP_MessageType` headers.

- **Externalized parameters list and their descriptions**
    - SEDA_MAIN_QUEUE: The name of the JMS queue used for message exchange.
    - Retention Threshold 4 Alerting: Threshold for retention alerting (JMS Adapter).
    - Expiration Period: Expiration period for JMS messages.
    - Number of Concurrent Processes: The number of concurrent processes for the JMS receiver adapter.
    - Maximum Retry Interval: Maximum retry interval for the JMS receiver adapter.
    - Retry Interval: Retry interval for the JMS receiver adapter.
    - MaxRetries: Max number of retries before discarding message.

- **DataStore / JMS Dependency**
Yes