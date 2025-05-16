**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender1: SFTP Sender] --> B(Start Event)
    B --> C{JavaScript 1}
    C --> D[Transform Filename: Groovy Script]
    D --> E[Message Mapping 1]
    E --> F[Send 1: SFTP Receiver]
    F --> G(End Event)
    F --> H[Receiver: SFTP Receiver]
\`\`\`

**Functional Summary**
- **Brief description of the iFlow**
This iFlow retrieves a file from a source SFTP server, transforms the filename using a Groovy script, optionally applies a message mapping and sends the file to a destination SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender1: SFTP Adapter, EndpointSender
    - Receiver: SFTP Adapter, EndpointRecevier

- **Key steps**
1.  Receive file from source SFTP server (Sender1).
2.  Execute a JavaScript script.
3.  Transform the filename using Groovy script.
4.  Perform optional Message Mapping.
5.  Send the file to the target SFTP server (Receiver).

- **Message transformation**
    - Transform Filename: Groovy Script (transformFilename.groovy)
    - Message Mapping: Uses Message Mapping component, but no specific mapping is defined.

- **Externalized parameters list and their descriptions**
    - `host`: Hostname of the destination SFTP server.
    - `user_uk2`: Username for the destination SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found