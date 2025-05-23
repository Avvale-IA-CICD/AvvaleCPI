**iFlowId**: Test_-_Poll_SFTP_using_HTTPS_call_as_trigger - **iFlowVersion**: 1.0.4

**Mermaid Diagram**
```mermaid
graph LR
    POSTMAN --> HTTPS[Start Event]
    HTTPS -- HTTPS Adapter --> SetFileName_A
    SetFileName_A[Set FileName A_*.txt] --> ReadProcess_A
    ReadProcess_A[Read and process every file A_*.txt] --> SetFileName_B
    SetFileName_B[Set FileName B_*.txt] --> ReadProcess_B
    ReadProcess_B[Read and process every file B_*.txt] --> End

    subgraph Process_15 [Iterative process: read every file + processing]
    Start1 --> SetBodyBlank
    SetBodyBlank[Set body blank] --> PollEnrich
    PollEnrich -- SFTP Adapter --> CheckFile
    CheckFile[Check file contents] --> ExclusiveGateway
    ExclusiveGateway -- Yes --> ProcessFile
    ProcessFile[Process file] --> End1
    ExclusiveGateway -- No --> End2
    end

    ProcessFile --> SendRequestCatcher
    SendRequestCatcher[Send to request-catcher] -- HTTP Adapter --> REQ_CAT
```
**BPMN Diagram**

![BPMN Diagram](./Test_-_Poll_SFTP_using_HTTPS_call_as_trigger-1.0.4.png "BPMN Diagram")

**Functional Summary**
- **Brief description of the iFlow**
This iFlow is triggered by an HTTPS call. It then polls an SFTP server for files, processes each file, and sends data to a request catcher service (REQ_CAT) via HTTP.

- **Involved systems with Adapters Type and Endpoint Type**
    - POSTMAN - HTTPS - EndpointSender
    - SFTP - PollingSFTP - EndpointSender
    - REQ_CAT - HTTP - EndpointRecevier

- **Key steps**
 1. Receives HTTPS request.
 2. Sets the filename property (A_*.txt).
 3. Polls SFTP server to reads files matching the filename property (A_*.txt).
 4. Sets the filename property (B_*.txt).
 5. Polls SFTP server to reads files matching the filename property (B_*.txt).
 6. For each file found in SFTP server runs:
     - Sets body blank using groovy script.
     - Reads a file content from SFTP server.
     - Checks file content using groovy script.
     - If file content validation is true, send file to Process file
     - Process the file and sends to request-catcher service (REQ_CAT).

- **Message transformation**
    - Set filename property A_*.txt using content modifier.
    - Set filename property B_*.txt using content modifier.
    - Sets body blank using groovy script.
    - Checks file content using groovy script.

- **Externalized parameters list, configured values and their descriptions**
Not Found

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found