Include in this file the 7 steps for Iteration 3


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

