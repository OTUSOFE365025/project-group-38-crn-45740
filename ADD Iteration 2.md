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

Step 6: Sketch Views and Record Design Decisions

Initial domain model

<img width="834" height="633" alt="Screenshot 2025-11-19 at 8 29 59 AM 2" src="https://github.com/user-attachments/assets/b5fbb1a6-5225-4ee4-847b-2d4754649302" />

Domain objects associated with the use case model

<img width="1048" height="481" alt="Screenshot 2025-11-19 at 9 00 42 AM" src="https://github.com/user-attachments/assets/d0e63d64-2989-446d-beec-8a05e05e589f" />

Modules that support the Primary Use Case


<img width="611" height="2211" alt="Diagram drawio" src="https://github.com/user-attachments/assets/3c98a3f5-b9b1-4a7e-ac14-73f7aa2f9b0a" />

**Element                                           -------------------------- Responsibility**

| AdminDashboardView | Displays administrative information such as policies, integrations, and usage statistics. This component embodies both UI components and UI process components from the reference architecture. |
| :---- | :---- |
| MaintainerDashboardView | Displays system monitoring information including errors, logs, backups, and model configuration. This component updates its view when new maintenance data is received from the system. |
| DashboardController | Responsible for providing necessary information to the presentation layer for both administrative and maintenance dashboards |
| ClientRequestManager | Responsible for communication with the server-side logic |
| Admin Service | Provides a facade that receives administrative request from the client |
| AnnouncementService | Provides a facade for handling and distributing campus-wide announcements. |
| MonitoringService | Provides a facade that receives mentoring-related requests from clients |
| PolicyController | Contains business logic related to managing global system policies |
| IntegrationController | Contains business logic related to maintaining institutional policies. |
| AnnouncementController | Contains business logic related to the creation and processing of announcement |
| MonitoringController | Contains business logic related to system errors, logs and performance metrics. |
| BackUpController | Contains business logic related to data backup management. |
| AiModelConfigurationController | Contains business logic related to configuring and maintaining AI model parameters and API keys |
| PolicyDataMapper | Responsible for persistence operations (CRUD) related to global policies. |
| IntegrationDataMapper | Responsible for persistence operations (CRUD) related to institutional integrations |
| AnnouncementDatMapper | Responsible for persistence operations (CRUD) related to announcement |
| SystemErrorDataMapper | Responsible for persistence operations (CRUD) related to system error records |
| LogDataMapper | Responsible for persistence operations (CRUD) related to log entries |
| BackupDataMapper | Responsible for persistence operations (CRUD) related to system backups. |
| AIConfigDataMapper | Responsible for persistence operations (CRUD) related to AI Model Configuration settings |


Sequence diagram for UCI-5.

This diagram  shows the sequence of steps for UC-5, where an administrator logs in and opens the system dashboard. After the dashboard loads, it asks the server for the list of current global policies. The request is handled through the AdminService, and the PolicyController retrieves all the policies from the PolicyDataMapper. Once the data is returned, the dashboard displays the policies so the administrator can review and manage them.

<img width="784" height="517" alt="Screenshot 2025-11-19 at 11 56 08 AM" src="https://github.com/user-attachments/assets/fa7e69a1-b78d-42e2-82d6-50cf4b62a9a4" />




| Element | Methods |
| :---- | :---- |
| Dashboard Controller | **void requestPolicies()** Forwards the request to the client-side request manager |
| ClientRequestManager | **Response sendRequest** Responsible for communication with the server-side logic. |
| AdminService | **PolicyList getPolicies()** Receives requests from clients and forwards them to the policy controller. |
| PolicyController | **PolicyList fetchPolicies()** Contains the business logic for retrieving policies. |
| PolicyDataMapper | **PolicyList retrieveAll()** Performs persistence operations (CRUD) for policies. |
| AdminDashboardView | **boolean initialize()** Opens the administrative dashboard so users can interact with system-management features. **void loadPolicies()** Requests the current policies to populate the view. |



Sequence Diagram for UCI-7

This diagram  illustrates the sequence for UC-7, where a system maintainer checks the system’s health. After logging in, the maintainer opens the monitoring dashboard, which immediately requests the most recent error logs from the server. The MonitoringService forwards this to the MonitoringController, which collects the latest errors from the SystemErrorDataMapper. The results are then sent back to the dashboard so the maintainer can review any issues and take action if needed.


<img width="923" height="601" alt="Screenshot 2025-11-19 at 12 02 55 PM" src="https://github.com/user-attachments/assets/4b4ceed7-8199-44cf-9c34-9898be0f0bbf" />



| Element | Methods |
| :---- | :---- |
| Dashboard Controller | **void requestRecentErrors()**Forwards a request to the monitoring service through the client request manager.  |
| ClientRequestManager | **Response sendRequest**Responsible for handling monitoring requests and communicating with the server side.  |
| MonitoringService | **ErrorList getRecentErrors()** Entry method exposed in the monitoring service interface. |
| MonitoringController | **ErrorList fetchRecentErrors(int limit)** Contains business logic for retrieving recent system errors. |
| SystemErrorDataMapper | **ErrorList retrieveRecent(int limit)** Returns the most recent system error entries from the database. |
| Monitoring DashboardView | **boolean initialize()** Opens the maintenance dashboard so the system maintainer can view system-health features. **void loadRecentErrors()** Requests recent error data to display on the dashboard. |


Step 7: Perform Analysis of Current Design and Review Iteration

This iteration clarified how UC-5, UC-6, and UC-7 are supported by identifying the key modules and placing them in the correct layers. These decisions also allowed the team to outline initial work assignments for CRN-3. A few new architectural concerns were discovered and added to the Kanban board, while drivers fully addressed in Iteration 1 were removed from this round.



| Not addressed | Partially addressed | Completely addressed | Design decision made during the iteration |
| :---- | :---- | :---- | :---- |
|  |  | UC-5 | Modules across the layers and preliminary interfaces to support this use case have been identified |
|  |  | UC-6 | Modules supporting announcement creation and distribution have been identified |
|  |  | UC-7 | Modules for system monitoring, log retrieval, backup management, and AI configuration have been identified |
|  | QA-1 |  | Only the components supporting UC-7’s monitoring path have been analyzed in this iteration. |
|  | QA-2 |  | Elements supporting backup, error handling, and monitoring were identified, but failover-level decisions remain for later iterations. |
| QA-3 |  |  | No relevant design decisions were made in this iteration. |
|  |  | QA-4 | The elements supporting UC-5 and UC-7 (dashboard views and controllers) have been identified. |
|  | QA-5 |  | Authentication and authorization were considered indirectly through admin/maintainer roles, but no dedicated security module was refined here. |
|  |  | QA-8 | Facade, adapter, pub-sub, and proxy patterns were chosen to structure module interaction and prepare for integration changes. |
| CON-1 |  |  | No relevant decisions made in this iteration. |
| CON-4 |  |  | No model-performance decisions were included here. |
|  |  | CON-5 | Modules for reading/writing policies, errors, logs, and backups were identified and mapped to the institutional database. |
|  |  | CON-6 | Modules responsible for backups and model configuration have been identified with the assumption they store data securely. |
|  |  | CON-8 | IntegrationController and related mappers were identified as the modules responsible for interacting with LMS/SIS/calendar APIs. |
|  | CON-10 |  | Announcement and usage-report modules support retrieving official data sources, but enforcement logic will be addressed later. |
|  | CRN-2 |  | Some additional technologies (pub-sub, proxy, adapter) were considered based on the team’s knowledge. |
|  |  | CRN-3 | Modules associated with all three selected use cases were identified, and a work-assignment matrix can now be created. |
|  | CRN-4 |  | The architectural concern of maintainability and module testability was touched on through separation of controllers and data mappers. |
|  |  | CRN-5 | Integration-related modules (IntegrationController, IntegrationDataMapper) were identified to handle external system communication concerns. |



