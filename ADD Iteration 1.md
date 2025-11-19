# Step 1: Review Inputs

| **Category**                  | **Details** |
|-------------------------------|-------------|
| **Design purpose**            | This is a greenfield system from a mature domain, and the purpose is to design the support for an AI-powered Digital Assistant platform (AIDAP). |
| **Primary functional requirements** | From the use cases, the primary ones were determined:<br><br>**UC-1:** Because it directly supports the core business<br><br>**UC-2:** Because it directly supports the core business<br><br>**UC-4:** Because it directly supports the core business<br><br>**UC-7:** Because of the technical issues related with QA-8, QA-9, and QA-10 |
| **Constraints**               | From the constraints, we decided to use all of them as drivers. |
| **Architectural concerns**    | From the architectural concerns, we decided to use all of them as drivers. |

Quality attribute scenario:
| **Scenario ID** | **Importance** | **Difficulty of implementation according to the architect** |
|-------------|------------|-------------------------|
| QA-1 | High | High |
| QA-2 | High  | High |
| QA-3 | Medium | High |
| QA-4 | Medium | Medium |
| QA-5 | High | Medium |
| QA-6 | Medium | Medium |
| QA-7 | Medium | Low |
| QA-8 | Low | Low |
| QA-9 | Low | Low |
| QA-10 | Low | Low |

# Step 2: Goals of the first Iteration
The goal of the first iteration is to define the overall architectural system, otherwise stated in CRN-1
While we are designing the general system, we should keep in mind some drivers along the way

QA-1: Performance<br><br>
QA-2: Availability<br><br>
QA-3: Scalability<br><br>
QA-4: Usability<br><br>
QA-5: Security<br><br>
QA-6: Portability<br><br>
QA-8: Modifiability<br><br>
CON-2: The system must be accessible via web, mobile, and voice assistants across major platforms. 

# Step 3: Choose One or More Elements of the System to Decompose
Since we are designing the overall system in the initial iteration, we will consider the system as a whole.

# Step 4: Choose one or more design concepts that satisfy the inputs considered in the iteration
For the entire system, we chose a Rich Internet Application reference architecture for the following reasons:
By using a RIA, we can easily create a cross-platform application (CON-2, QA-6) with a rich user interface (QA-4). By using this architecture, we can implement a scalable backend server capable of replying to user queries in a timely manner, even under load (QA-1, QA-3). This architecture supports an easily modifiable server-side through a layered-architecture, giving it great modifiability (QA-8). Lastly, through the RIA, in the event of a data source failure, the client can cache recent data, giving the server side ample time to gracefully retry connection (QA-2).

In terms of other alternatives, we decided to reject them for these reasons:
| **Alternative** | **Reason for Discarding** |
|--------------------------|----|
| Rich Client Application | This architecture is made to work in a stand-alone style, which goes against QA-6. |
| Web Application | This architecture has less support for appealing and responsive UIs, which goes against QA-4. It also does not have proper isolated storage for caching like RIA has, which goes against QA-2. |
| Mobile Application | This architecture only supports mobile devices, which goes against QA-6.|


# Step 5: Instantiate Architectural Elements, Allocate Responsibility, and Define Interfaces
| Component | Description |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI Chatbot | The chatbot implementation to be used by the system. |
| Rich-UI/Presentation | The overall frontend UI, containing the dashboards for lecturers, students, system maintainers, and administrators. |
| Notification System | The push notification system that the system will use. |
| Database | The institutional database used by the system to store chat history, user data, and such. |
| Data Access | The component that will let the system access data from the institutional database. |
| External System Integration (Facade) | The component that will get data from external systems such as calendars, LMS, and university sites. |
| Service Interface | The connecting component between the client and the server. Passes information between the client and the server-side business layer. |
| Authentication (SSO) | A cross-cutting component that will handle authentication in every layer of the server side. |
| Client Isolated Storage | A storage solution where client-side information gets stored, like authentication and caching. |
| Operational Management | A cross-cutting component that will handle tasks like logging. |
| Client Processing | A client-side component that will handle tasks like caching and dynamic UI rendering. |

The results of these instantiation decisions are recorded in the next step. In this initial iteration, the details are not yet well known. The following iterations will further develop their functionality. The same can be said for interfaces. 

# Step 6: Sketch views and record design decisions
Reference Architecture
<br><br>
<img width="780" height="1106" alt="image" src="https://github.com/user-attachments/assets/093759c7-9a3c-4be3-a6f8-3c2457b87b0b" />
<br><br>
The diagram represents an overview into the position of components mentioned in the previous steps, in the chosen reference architecture.
<br><br>
Depoyment Diagram:
<br><br>
<img width="1285" height="691" alt="image" src="https://github.com/user-attachments/assets/0c380f89-04b8-4fed-8268-c1d4e3f25e2a" />

A basic deployment diagram for the chosen architecture mentioned above. The database and external systems were included.

| **Element** | **Responsability** |
| :---- | :---- |
| User Device | Represents the user’s device which runs the web browser hosting the client-side application. |
| Application Server | The server that hosts content for the client side to access. |
| Database Server | The server hosting the institutional DBMS. |
| Authentication Service | The external service hosting the institutional authentication system. |
| LMS | The external server hosting the university’s LMS. |


# Step 7: Perform analysis of the current design and review the iteration goal and design objectives
| Not Addressed  | Partially Addressed  | Completely Addressed | Design Decisions made during the iteration |
| :---- | :---- | :---- | :---- |
|  |  | UC-1 | The AI chatbot component was implemented in the reference architecture.  |
|  | UC-2 |  | The external services were portrayed with a facade interface in the reference architecture |
|  | UC-3 |  | The notifications component was implemented in the reference architecture. |
|  |  | UC-4 | The general UI for the lecturers was mentioned in the Rich UI/Presentation component in our reference architecture. |
|  | UC-5 |  | An interface of operational management was initiated in the reference architecture |
|  | UC-6 |  | The AI assistant was mentioned as the AI chatbot component, and notifications are implemented as a component as well. |
|  | UC-7 |  | An interface of operational management was initiated in the reference architecture |
| QA-1 |  |  | No relevant decisions made. |
|  | QA-2 |  | The chosen reference architecture has the client isolated storage, which will store cache in case of limited connectivity.    |
| QA-3 |  |  | No relevant decisions made. |
|  |  | QA-4 | Selected reference architecture supports a strong, rich UI  |
|  |  | QA-5 | The selected reference architecture supports authentication, which will be provided by the institutional sign in. The data will also be safe, since the system uses the institutional database. These 2 are reflected as components as well. |
|  |  | QA-6 | Selected reference architecture supports multiplatform |
| QA-8 |  |  | No relevant decisions made. |

