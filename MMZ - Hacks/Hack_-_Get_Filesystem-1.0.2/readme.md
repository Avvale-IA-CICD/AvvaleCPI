**iFlowId**: Hack_-_Get_Filesystem - **iFlowVersion**: 1.0.2

**Mermaid Diagram**
```mermaid
graph LR
    A[Start Timer 1] --> B(Get FileSystem)
    B --> C[End]
```
**Functional Summary**
- **Brief description of the iFlow**
  This iFlow is triggered by a timer event. It then executes a Groovy script named "script1.groovy" and then ends.

- **Involved systems with Adapters Type and Endpoint Type**
  - None. The iFlow only uses a timer and a Groovy script.

- **Key steps**
 1. Start: The iFlow starts based on a timer event. The timer is configured to fire immediately.
 2. Get FileSystem:  Executes the Groovy script "script1.groovy".
 3. End: The iFlow execution ends.

- **Message transformation**
  - None. There are no explicit message mapping or content modifier steps.

- **Externalized parameters list and their descriptions**
  - None. The iFlow does not have any externalized parameters.

- **DataStore / JMS Dependency**
  Not Found

- **Cloud Connector Dependency**
  Not Found

- **Common Scripts Dependency**
  List of scripts: script1.groovy

- **ProcessDirect ComponentType Dependency**
  Not Found