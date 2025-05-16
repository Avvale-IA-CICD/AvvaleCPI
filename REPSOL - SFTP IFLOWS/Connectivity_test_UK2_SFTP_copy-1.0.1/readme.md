**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender1: SFTP Sender] --> B(Start Event)
    B --> C{JavaScript 1}
    C --> D{Transform Filename}
    D --> E{Message Mapping 1}
    E --> F[Send 1: SFTP Receiver]
    F --> G(End Event)
    F --> H[Receiver: SFTP Receiver]
\`\`\`

**Functional Summary**
- **Brief description of the iFlow**
This iFlow retrieves a file from a source SFTP server, transforms the filename, performs a message mapping and sends the file to a target SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender1: SFTP Adapter, Endpoint Sender
    - Receiver: SFTP Adapter, Endpoint Receiver

- **Key steps**
 1. Receive file from source SFTP server (Sender1).
 2. Execute a JavaScript to prepare the file for filename transformation.
 3. Transform the filename using a Groovy Script.
 4. Perform a Message Mapping.
 5. Send the file to the destination SFTP server (Receiver).

- **Message transformation**
    - Transform Filename: Groovy Script (transformFilename.groovy)
    - JavaScript 1: JavaScript script
    - Message Mapping 1: Message Mapping

- **Externalized parameters list and their descriptions**
    - `host`: Hostname of the destination SFTP server.
    - `user_uk2`: Username for the destination SFTP server authentication.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found