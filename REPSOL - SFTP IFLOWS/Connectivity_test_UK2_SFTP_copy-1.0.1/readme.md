**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender1 SFTP] --> B(Start Event)
    B --> C{JavaScript 1}
    C --> D{Transform Filename}
    D --> E{Message Mapping 1}
    E --> F(Send 1)
    F --> G[Receiver SFTP]
    F --> H(End Event)
\`\`\`

**Functional Summary**
- **Brief description of the iFlow**
This iFlow retrieves a file from an external SFTP server, transforms the filename, performs a message mapping, and sends the file to another SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender1: SFTP Adapter (Endpoint Sender)
    - Receiver: SFTP Adapter (Endpoint Receiver)

- **Key steps**
1. Receive file from Sender1 SFTP server.
2. Execute a JavaScript script.
3. Transform the filename using a Groovy script (`transformFilename.groovy`).
4. Perform a message mapping.
5. Send the file to Receiver SFTP server.

- **Message transformation**
    - Filename transformation using `transformFilename.groovy` script.
    - Message mapping (mapping name and URI are empty).

- **Externalized parameters list and their descriptions**
    - `host` (Receiver): Hostname of the SFTP server for Receiver.
    - `user_uk2` (Receiver): Username for the SFTP server authentication for Receiver.
    - No username for Sender1 SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found