| ID | Quality Attribute | Scenario | Associated use case | 
| :--- | :---: | :---: | ---: |
| QA-1 | Performance | The system shall respond to queries within 2 seconds on average under normal load. | UC-1, UC-4, UC-6 |
| QA-2 | Availability/Reliability | The system shall remain available 99.5% of the time per month. It shall also support offline cashes of recent responses for limited connectivity, and have automatic fail-over and backup recovery. In the event of a data source failure, it shall gracefully handle it and retry connection. | All |
| QA-3 | Scalability | Handle up to 5000 concurrent users | All |
| QA-4 | Usability | The system shall have an intuitive UI consistent with conversational design best practices. | UC-2, UC-4, UC-5, UC-7 |
| QA-5 | Security | User data shall be protected by ensuring that student-specific data is visible only to the authenticated user, and the system must comply with institutional privacy policies. | UC-1, UC-2, UC-4 | 
| QA-6 | Portability | The system shall be accessible on mobile, web, and voice-assistant devices. | UC-1, UC-2, UC-4, UC-6 |
| QA-7 | Security | The system shall ensure that only authorized lecturers can modify course data. | UC-4 | 
| QA-8 | Modifiability | The system shall be easily extensible to integrate new AI services or external data sources. | UC-1, UC-4, UC-6 |
| QA-9 | Interoperability | The system shall use standard APIs (REST or GraphQL). | All |
| QA-10 | Data Integrity | The system shall maintain data integrity and consistency across systems. | All |
