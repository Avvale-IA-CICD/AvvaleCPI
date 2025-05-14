**iFlowId:** SEDA_Model_-_Single_Queue_-_Restart_and_Discard - **iFlowVersion:** 1.0.0

**Functional Summary**

- **Brief description of the iFlow**
This iFlow implements a SEDA (Staged Event-Driven Architecture) pattern with a single JMS queue, demonstrating message processing across multiple steps. It includes error handling with exception subprocesses and discarding messages based on retry count or unknown processing step. The iFlow is triggered either by an HTTPs call or a JMS message.

- **Involved systems**
    - SQUEUE
    - RQUEUE
    - Postman

- **Used Adapters**
    - JMS
    - HTTPS

- **Key steps**
 1.  Receive message from SQUEUE (JMS) or Postman (HTTPS).
 2.  Set initial headers and save the initial message (Step 0).
 3.  Route the message based on the `Step` property.
 4.  Execute Step 1, Step 2, or Step 3 based on the routing. Each step involves:
        - Preparing the step with message enrichers and setting properties.
        - Calling a local integration process to execute the step's logic.
        - Sending the message to the next step's queue (RQUEUE).
        - Logging a custom status for step completion.
 5.  If the `Step` property is unknown, the message is discarded.
 6.  If the message exceeds the maximum retry count, it is discarded and logged.
 7. Log any exceptions during the process asynchronously.

- **Message transformation**
    - Content enrichers are used to add headers, properties, and message body content.
    - Groovy scripts are utilized for logging discarded messages and handling exceptions.
    - Message routing occurs based on the value of the `Step` property.

- **Externalized parameters list and their descriptions**
    - `SEDA_MAIN_QUEUE`: Name of the JMS queue used for message exchange.
    - `Number of Concurrent Processes`: Number of concurrent processes for the JMS adapter.
    - `Maximum Retry Interval`: Maximum retry interval for the JMS adapter.
    - `Retention Threshold 4 Alerting`: Retention Threshold for Alerting for the JMS adapter.
    - `Expiration Period`: Expiration Period for the JMS adapter.
    - `Retry Interval`: Retry Interval for the JMS adapter.
    - `MaxRetries`: Maximum number of retries before discarding the message.

- **DataStore / JMS Dependency**
Yes

**Mermaid Diagram**

```mermaid
graph LR
    subgraph Initial_Message_Handling
        Postman[Postman (HTTPS)] --> StartEvent6((Start Event))
        SQUEUE[SQUEUE (JMS)] --> StartEvent2((Start Event))
        StartEvent6 --> SetHeaders0[Set Headers]
        StartEvent2 --> Reprocess{Reprocess?}
        Reprocess -- Yes --> Step_Check{Step?}
        Reprocess -- Discard --> DiscardMaxRetries[Discard Max Retries]
        DiscardMaxRetries --> LogDiscardedMessageMax[Log Discarded Message]
        SetHeaders0 --> SaveInitialMsg[Save Initial Msg]
        SaveInitialMsg --> CustomStatus0[Custom Status]
        CustomStatus0 --> EndEvent5((End Event))
    end
    subgraph Route_Process
    Step_Check -- Step1 --> SetHeaders1[Set Headers]
    Step_Check -- Step2 --> SetHeaders2[Set Headers]
    Step_Check -- Step3 --> SetHeaders3[Set Headers]
    Step_Check -- Unknown --> CustomStatusUnknown[Custom Status]
    SetHeaders1 --> Step1[Step 1]
    SetHeaders2 --> Step2[Step 2]
    SetHeaders3 --> Step3[Step 3]
    Step1 --> NextStep1[Next Step]
    Step2 --> NextStep2[Next Step]
    Step3 --> NextStep3[Next Step]
    NextStep1 --> CustomStatus1[Custom Status]
    NextStep2 --> CustomStatus2[Custom Status]
    NextStep3 --> CustomStatus3[Custom Status]
     end   
    
    CustomStatusUnknown --> LogDiscardedMessageUnknown[Log Discarded Message]
    LogDiscardedMessageUnknown --> DiscardUnknown((Discarded Unknown))
    CustomStatus1 --> EndEvent((End Event))
    CustomStatus2 --> EndEvent
    CustomStatus3 --> EndEvent
    NextStep1 --> RQUEUE
    NextStep2 --> RQUEUE
    NextStep3 --> RQUEUE
    LogDiscardedMessageMax --> DiscardedMaxRetries((Discarded MaxRetries))
   

   subgraph Step1_Details
        Step1 --> PrepareStep2[Prepare Step 2]
        PrepareStep2 --> EndStep1((End Step 1))
    end

    subgraph Step2_Details
        Step2 --> PrepareStep3[Prepare Step 3]
        PrepareStep3 --> EndStep2((End Step 2))
    end

    subgraph Step3_Details
        Step3 --> TestThrowException[Test Throw Exception]
        TestThrowException --> EndStep3((End Step 3))
    end

    subgraph Exception_Handling
     Step1 --- ExceptionSubprocess1[Exception Subprocess 1]
     Step2 --- ExceptionSubprocess2[Exception Subprocess 2]
     Step3 --- ExceptionSubprocess3[Exception Subprocess 3]
    end
    

     style StartEvent6 fill:#f9f,stroke:#333,stroke-width:2px
     style Postman fill:#ccf,stroke:#333,stroke-width:2px
     style Step1 fill:#ccf,stroke:#333,stroke-width:2px
     style Step2 fill:#ccf,stroke:#333,stroke-width:2px
     style Step3 fill:#ccf,stroke:#333,stroke-width:2px
     style PrepareStep2 fill:#ccf,stroke:#333,stroke-width:2px
     style PrepareStep3 fill:#ccf,stroke:#333,stroke-width:2px
     style TestThrowException fill:#ccf,stroke:#333,stroke-width:2px
```