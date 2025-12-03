Add iteratation 3:Addressing Quality attribute scenario driver (QA-2)

Step 2:
Iteration 3 targets the Availability quality attribute, QA-2. The selected scenario is: If a system component or external service fails, the system must remain operational and recover automatically within 30 seconds. In this iteration, the focus is on adding mechanisms for failure detection, graceful degradation, and failover to guarantee that the system meets the uptime and reliability requirements.


Step 3 Choose One or More Elements of the System to Refine
Refined elements for this availability scenario include the following:
Application Server:  which must detect failures and continue providing responses through fallback or cached data.
Institutional Database Server: that needs to support failover and be always available for system operation during any failure.

Step 4: The design concepts used in this iteration are the following:
Step 4: The design concepts used in this iteration are the following:

| Design Decisions and Location | Rationale and assumption |
| :---- | :---- |
| **Introduce replicated instances of the application server and database server behind a failover mechanism** | By running multiple instances of the application server and using a primary–replica setup for the institutional database, the system can switch to a healthy instance if one fails. This helps the platform stay available during faults and supports the required recovery time for QA-2 and CON-9 |
| **Add health-check and circuit-breaker logic to the service interface layer.**  | The service interface continuously checks the status of backend services (AI model, LMS, SIS, calendar, database). When repeated failures are detected, the circuit breaker temporarily stops sending requests to the failing service and either routes to a replica or returns cached/partial data. This avoids cascading failures, improves perceived availability, and supports both QA-2 (availability) and QA-1 (performance). |


Step 5 Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces


| Design decisions and location | Rationale |
| :---- | :---- |
| **Deploy a second Application Server instance behind a load balancer** | Running at least two application server instances allows the system to continue serving requests if one instance fails. The load balancer distributes incoming traffic across healthy instances and stops sending requests to a failing node once health checks fail. This supports the availability goal (QA-2) and the uptime constraint (CON-9). |
| **Configure the Institutional Database Server as a primary–replica setup with automatic failover** | Using a primary database with one or more replicas enables the system to switch to a replica if the primary becomes unavailable. This helps the platform remain operational during database failures and reduces recovery time after an outage. It also supports QA-2 and QA-10 by keeping data consistent and available |
| **Introduce health-check interfaces between the load balancer and both the Application Server and Database endpoints** | Health checks allow the load balancer and infrastructure to detect when an instance or endpoint is unhealthy and remove it from the active pool. This avoids routing user requests to failed components and is key to meeting the “recover within 30 seconds” objective. This decision also introduces a new architectural concern: **CRN-X: managing state and session information across replicated application servers.** |


Step 6: 
Refined Deployment Diagram
<img width="841" height="431" alt="Deployment drawio" src="https://github.com/user-attachments/assets/6bddd112-641a-46a9-8788-7af03d082a99" />

The refined deployment diagram for this iteration illustrates the introduction of redundancy into the system to support the selected availability scenario. In this view, both the Application Server and the Institutional Database Server now appear in replicated form, managed through a load balancer that routes incoming client requests only to healthy server instances.



| Element | Responsibility |
| :---- | :---- |
| LoadBalancer | Distributes incoming requests across the replicated application servers. Removes failed servers from rotation based on health-check results. Presents a single stable endpoint to clients. |
| ReplicaApplicationServer | Provides the functional backend logic for chat queries, dashboards, and notifications. Multiple replicas ensure the system continues operating if one instance fails |
| ReplicaDatabaseServer | Provides a synchronized copy of institutional data. Serves as the failover target when the primary database becomes unavailable. |

Sequence diagram
<img width="631" height="372" alt="sequence8 drawio" src="https://github.com/user-attachments/assets/ad0bcb85-4763-4911-b2f4-bcfa2eee1174" />

Step7:Perform Analysis of Current Design and Review Iteration
The design decisions for this iteration were centered on enhancing the availability of the system QA-2, which includes the refinement of the system behavior in case of a database failure. Introduction of a load balancer managing database failover and a replicated database instance provide the backbone for supporting operation during outages. The status of main drivers considered for this iteration is summarized in the following table.



| Not addressed | Partially addressed | Completely addressed | Design Decisions Made During the Iteration |
| :---- | :---- | :---- | :---- |
|  | QA-1 |  | Routing database queries via a load balancer reduces the retry burden on the application server during failures, helping maintain stable performance. Still, specific performance technologies, such as connection pooling strategy, have not been selected yet.  |
|  | QA-2 |  | A load balancer and a replicated database improve the system's availability in the event of primary-database failures by redirecting the queries. The driver is marked as "partially addressed," because some mechanisms for replica promotion and recovery timing are not yet fully defined.  |
|  | QA-3 |  |  Scalability mechanisms (e.g., adding more app servers, horizontal scaling strategies) were introduced in this iteration.  |
| QA-4 |  |  |  No UI-related OR user-experience decisions were made during this iteration.  |
| CON-1 |  |  | This iteration did not address authentication or security integration |
| CON-4 |  |  | No performance-tuning decisions were introduced. |
| CON-5 |  |  | The design continues to use only institutional database servers, including replication—fully aligned with this constraint |
| CON-6 |  |  | No new encryption or secure-transfer mechanisms were addressed |
| CRN-2 |  |  | This iteration focused solely on database availability and did not address AI-related concerns. |
| CRN-4 |  |  | No security-filtering or privacy improvements were introduced. |
| CRN-5 |  |  | Although failover was introduced for the database layer, no decisions were made for LMS/SIS/Calendar availability |



**ATAM Utility Tree**

Below is the utility tree containing the 10 QAs described. 
<img width="2012" height="961" alt="image" src="https://github.com/user-attachments/assets/aea17294-cbdb-4bd4-9d08-028e1494d4bf" />


**ATAM Risk Assessment:**

**Sensitivities:**

- S1: The level of failover is sensitive to the time it takes to switch to the failover system and might not be able to reach under 30 seconds  
- S2: Boot time is sensitive to the number of requests the load balancer has piled up during failure  
- S3: Data integrity may be violated if the data isn't properly synchronized between database and application servers.

**Trade-Offs**

- T1: Load balancers and multiple application servers and databases improve availability, but can be costly to run  
- T2: Depending on distributed database setup, to improve availability and consistency, write performance may be impacted heavily.  
- T3: Load balancing and replication improve robustness and reliability, at the cost of increased complexity, and a decrease in maintainability.

**Risks:**

- R1: If a single point of a load balancer is used, the total system relies on that single point of entry, and it can be detrimental if it shuts down   
- R2: If a single load balancer is used, every incoming request must go through it creating a bottleneck.  
- R3: Network latency created by slow replications can cause temporary inconsistencies between replicas, leading to issues such as race condition.  
- R4: Hardware failure in a replica can affect redundancy, if the other replicas can't handle the new load.

**Non-Risks**

- N1: Load balancers distribute request traffic, improving availability  
- N2: Multiple databases can offload queries, improving availability  
- N3: If a single resource fails, the replica can take over immediately and maintain availability  
- N4:If there is a multiple points of entry either in load balancing or servers and databases, there would be no single point of failure where it can take down the whole system

| Analysing Scenario |  QA-2 |  |  |  |
| :---- | :---- | :---- | :---- | :---- |
| **Scenario** | If a system component or external service fails, the system must remain operational and recover automatically within 30 seconds. |  |  |  |
| **Attributes** | Availability |  |  |  |
| **Stimulus**  | Failure |  |  |  |
| **Environment**  | Normal Operation |  |  |  |
| **Response** |  Recover automatically within 30 seconds |  |  |  |
| **Architecture Decisions** | **Sensitivity** | **Trade-off** | **Risk** | **Non-Risk** |
| AD 1: Load Balancers | S1, S2 | T1, T3 | R1, R2 | N1, N4 |
| AD 2: ReplicaApplicationServer | S1, S3 | T1, T3 | R3, R4 | N3. N4 |
| AD 3: ReplicaDatabaseServer | S1, S3 | T1, T2, T3 | R3, R4 | N2, N3, N4 |

