**iFlowId**: Connectivity_test_UK2_SFTP_copy - **iFlowVersion**: 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender SFTP] -- SFTP --> B(Start Event)
    B --> C{JavaScript 1}
    C --> D{Transform Filename}
    D --> E[Message Mapping 1]
    E --> F(Send 1)
    F -- SFTP --> G[Receiver SFTP]
    F --> H(End Event)
\`\`\`
**Functional Summary**
- **Brief description of the iFlow**
This iFlow reads a file from a sender SFTP server, performs filename transformation and message mapping, and then sends the file to a receiver SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender SFTP: Adapter Type: SFTP, Endpoint Type: EndpointSender
    - Receiver SFTP: Adapter Type: SFTP, Endpoint Type: EndpointRecevier

- **Key steps**
    1.  Receive file from sender SFTP server.
    2.  Execute a JavaScript step.
    3.  Transform the filename using a Groovy script ("transformFilename.groovy").
    4.  Perform message mapping.
    5.  Send the file to receiver SFTP server.

- **Message transformation**
    - Transform Filename: Groovy Script (transformFilename.groovy)
    - Message Mapping: Message Mapping is performed but no specific mapping is defined in the XML.

- **Externalized parameters list and their descriptions**
    - host (Receiver SFTP): Hostname/IP address of the receiver SFTP server.
    - user_uk2 (Receiver SFTP): Username for the receiver SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found