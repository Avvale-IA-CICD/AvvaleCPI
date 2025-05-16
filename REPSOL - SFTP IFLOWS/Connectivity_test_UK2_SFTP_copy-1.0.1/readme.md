**iFlowId:** Connectivity_test_UK2_SFTP_copy - **iFlowVersion:** 1.0.1

**Mermaid Diagram**
\`\`\`mermaid
graph LR
    A[Sender1: SFTP] --> B(Start Event);
    B --> C{JavaScript};
    C --> D{Transform Filename};
    D --> E{Message Mapping};
    E --> F(Send 1: SFTP);
    F --> G(End Event);
    F --> H[Receiver: SFTP];
\`\`\`

**Functional Summary**
- **Brief description of the iFlow**
The iFlow picks up a file from a source SFTP server, transforms the filename, performs a message mapping (although the mapping itself is not defined), and sends the file to a target SFTP server.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender1: SFTP (EndpointSender)
    - Receiver: SFTP (EndpointRecevier)

- **Key steps**
    1. The iFlow is triggered by a scheduled SFTP sender adapter that picks up a file named "test.txt" from the `/PRUEBA_POC` directory of the `b2b-test.repsol.com:122` SFTP server. The scheduler runs every 10 seconds.
    2. A JavaScript step is executed. The script is empty.
    3. The filename is transformed using a Groovy script named "transformFilename.groovy".
    4. A message mapping step is executed, but the message mapping details are not defined.
    5. The iFlow sends the file to the target SFTP server (`{{host}}`) in the `/UK2int/rpt/aviacao/in/AVR` directory.

- **Message transformation**
    - Filename transformation using "transformFilename.groovy" script.
    - Message Mapping (undefined).

- **Externalized parameters list and their descriptions**
    - `host`: Hostname/IP address of the receiver SFTP server.
    - `user_uk2`: Username to connect to the receiver SFTP server.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Yes

- **Common Scripts Dependency**
List of scripts: transformFilename.groovy

- **ProcessDirect ComponentType Dependency**
Not Found