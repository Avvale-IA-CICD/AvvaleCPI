**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender SFTP] --> B(Start Event)
    B --> C{JavaScript}
    C --> D[Transform Filename]
    D --> E[Message Mapping]
    E --> F(Send to Receiver SFTP)
    F --> G(End Event)
    G --> H[Receiver SFTP]
\`\`\`
**Functional Summary**
- **Brief description of the iFlow**
This iFlow retrieves a file from a sender SFTP server, performs a filename transformation and a message mapping, and then sends the resulting file to a receiver SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender: SFTP Adapter, Endpoint Sender
    - Receiver: SFTP Adapter, Endpoint Receiver

- **Key steps**
    1. Receive file from the sender SFTP server.
    2. Execute a JavaScript.
    3. Transform the filename using a Groovy script (transformFilename.groovy).
    4. Perform a message mapping.
    5. Send the processed file to the receiver SFTP server.

- **Message transformation**
    - Filename transformation using Groovy script: transformFilename.groovy
    - Message Mapping: Message Mapping 1

- **Externalized parameters list and their descriptions**
    - host: Hostname of the receiver SFTP server.
    - user_uk2: Username for the receiver SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found