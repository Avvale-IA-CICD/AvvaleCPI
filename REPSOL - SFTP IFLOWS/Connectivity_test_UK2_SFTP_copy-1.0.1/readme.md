**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender SFTP] --> B(Start Event)
    B --> C{JavaScript 1}
    C --> D{Transform Filename}
    D --> E{Message Mapping 1}
    E --> F[Send 1]
    F --> G(End Event)
    F --> H[Receiver SFTP]
\`\`\`

**Functional Summary**
- **Brief description of the iFlow**
The iFlow retrieves a file from an SFTP server, transforms the filename, performs a message mapping (though the mapping itself is not defined), and then sends the file to another SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender SFTP: SFTP Adapter, Endpoint Sender
    - Receiver SFTP: SFTP Adapter, Endpoint Receiver

- **Key steps**
    1.  Receive file from Sender SFTP server via a scheduled start event.
    2.  Execute a Javascript step.
    3.  Transform the filename using a Groovy Script "transformFilename.groovy".
    4.  Execute a Message Mapping step (no mapping defined).
    5.  Send the file to the Receiver SFTP server.

- **Message transformation**
    - Filename Transformation via Groovy Script: "transformFilename.groovy"
    - Message Mapping: Message Mapping step with no mapping details

- **Externalized parameters list and their descriptions**
    - `host`: Hostname of the Receiver SFTP server.
    - `user_uk2`: Username for the Receiver SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found