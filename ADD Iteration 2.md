# Step 5: Instantiate Architectural Elements, Allocate Responsibilities

To begin, we created a domain model for our driving use cases. Here are the initial domain objects per use case, using the nouns.

| Use Case | Objects |
| :---- | :---- |
| UC-5 | Administrator, system, global policies, institutional integrations, usage reports |
| UC-6 | Administrator, announcement, assistant |
| UC-7 | System maintainer, monitoring dashboard, error, permissions, logs, data backups, AI model, API keys |

Next, we filtered what objects there are and how they relate to the iteration 1 components.

| Domain Objects | Domain-Specific Component |
| :---- | :---- |
| User (with roles like administrator, system maintainer) | Authentication and Authorization |
| Usage reports | Reporting (Derived from Operational Management) / Institutional database |
| Global policies, institutional integrations | Policy and Integration (Derived from Operational Management) |
| Announcement | Chatbot Management (Derived from AI Chatbot) / Notification Service |
| Error, logs | System Monitoring & Logging (Derived from Operational Management) |
| Permissions (Value/object of user) | Authentication and Authorization |
| Monitoring dashboard, assistant (Not domain objects) | Rich UI and Presentation |
| Data Backups | Backup (New) |
| AI model (Entity in AI Configuration), API keys (Value/Object) | AI Configuration (Derived from AI Chatbot) |
| System | Too vague, or aren’t considered domain objects |

 

After relating them to components, here are the core domain objects with their attributes:

**User**:

Represents a User within the system. Will be used for authentication and authorization.

- userID  
- name  
- email  
- role (Administrator, System maintainer, Student, Lecturer)  
- permissions (List of allowed actions within the system)

**Announcement**:

Represents a campus-wide announcement to be issued by the administration. 

- announcementID  
- title  
- content  
- createdBy (user id of admin)  
- crationDate  
- recipientRole (admin, lecturer, students)

**GlobalPolicy**:

Represents applied rules to the system, may be related to many things like data policy, user policy, etc.

- policyID  
- name  
- description  
- status

**InstitutionalIntegration**:

Represents the connections to the external systems, like the LMS, calendar and university registrar for schedules.

- integrationID  
- name  
- connectionDetails  
- isConnected

**UsageReport**:

Represents a report summarizing statistical system usage and activity, for administration.

- reportID  
- generationDate  
- data

**SystemError**:

Represent an error that happened within the system, logged for maintenance.

- errorID  
- componentName  
- errorMessage  
- timestamp  
- severity

**LogEntry**:

Represents a record of system event, action or operation. Unlike errors, not necessarily bad. Used for maintenance and debugging.

- logID  
- componentName  
- message  
- timestamp  
- impact

**DataBackup**:

Represents a backed up, saved copy of system data for recovery purposes.

- backupID  
- initiatedBy (user id of the system maintainer)  
- status (complete or not)

**AIModelConfiguration**:

Represents the current AI model’s settings, parameters and configuration.

- modelName  
- version  
- settings  
- apiKey

**APIKey**:

Represents an API key for the external integrations and AI model.

- keyID  
- apiServiceName  
- value  
- status (in use or not)

Lastly, here are the entities and their relationships to each other.

| Source | Relationship | Target | Notes |
| :---- | :---- | :---- | :---- |
| User | Creates | Announcement | 1 (admin) \-\> 0..N announcements |
| Announcement | Targets | User | 1 \-\> 0..N users with matching role |
| GlobalPolicy | Governs | User / AIModelConfiguration/InstitutionalIntegration | 1 \-\> 0..N, they govern any target that they apply to. |
| UsageReport | Summarizes | Announcement, User | Reports on statistics for announcements and users, contains aggregate data. |
| SystemError | Logs | Components / System | Logs every error that happens within the system, so for every component. Can aggregate errors. |
| LogEntry | Records | Components / System | Logs actions that happen within the system, so every component. Can aggregate events. |
| DataBackup | Contains | Announcement, User, UsageReport, AIModelConfiguration | Composition: A data backup cannot exist independently, needs to contain data. |
| User | Initiates | DataBackup | 1 \-\> 0..N (user must be system maintainer) |
| User  | Views | SystemError / LogEntry | 1 \-\> 0..N (user must be system maintainer) |
| User  | Views | UsageReport | 1 \-\> 0..N (user must be administrator) |
| User | Modifies | GlobalPolicy / InstitutionalIntegration | 1 \-\> 0..N (user must be administrator) |
| User  | Modifies | AIModelConfiguration / APIKey  | 1 \-\> 0..N (user must be system maintainer) |
| AIModelConfiguration | Uses | APIKey | 1 \-\> 1 |
| InstitutionalIntegration | Uses | APIKey | 1 \-\> N (an integration may need multiple api endpoints with different keys) |

