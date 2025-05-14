**iFlowId**: Hack_-_Get_Filesystem_File - **iFlowVersion**: 1.0.2

**Mermaid Diagram**
```mermaid
graph LR
    Sender1[Sender1] --> HTTPS_Start[HTTPS /mmzhck/getFile]
    HTTPS_Start --> GroovyScript[Groovy Script Get File]
    GroovyScript --> End[End]
```
**Functional Summary**
- **Brief description of the iFlow**
This iFlow receives an HTTPS request, executes a Groovy script ("script1.groovy") to get a file, and returns the file content in the response.

- **Involved systems with Adapters Type and Endpoint Type**
    - Sender1: HTTPS Adapter, EndpointSender

- **Key steps**
    1. Receive HTTPS request at `/mmzhck/getFile`.
    2. Execute Groovy script `script1.groovy`.
    3. Send the result back to the caller.

- **Message transformation**
    - The Groovy script "script1.groovy" likely transforms the message by reading a file from the file system and setting the content in the message body.

- **Externalized parameters list and their descriptions**
    - There are no directly externalized parameters visible in the provided XML. The file path used by the `script1.groovy` script may be considered an implicit parameter if it can be configured externally.

- **DataStore / JMS Dependency**
Not Found

- **Cloud Connector Dependency**
Not Found

- **Common Scripts Dependency**
Not Found

- **ProcessDirect ComponentType Dependency**
Not Found