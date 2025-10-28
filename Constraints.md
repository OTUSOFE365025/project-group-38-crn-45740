| **ID** | **Constraint** | **Based On** | **Supports QA** |
|---------|----------------|---------------|-----------------|
| **CON-1** | The system should connect to the institution’s single sign-on (SSO) using SAML or OAuth2 to ensure secure login for students, lecturers, and admins. | Ensures secure login for all users. | QA-5 (Security), QA-9 (Interoperability) |
| **CON-2** | The system must be accessible via web, mobile, and voice assistants across major platforms. | Students use multiple devices; ensures portability and accessibility. | QA-6 (Portability), QA-4 (Usability) |
| **CON-3** | The system must handle up to 5000 users simultaneously. | Derived from anticipated user load and performance needs. | QA-3 (Scalability), QA-1 (Performance) |
| **CON-4** | Response time for AI queries must average ≤ 2 seconds under normal load. | Ensures efficient model serving. | QA-1 (Performance) |
| **CON-5** | Use only the institution’s official databases (e.g., SQL, Oracle). | Maintains data integrity and governance. | QA-10 (Data Integrity), QA-5 (Security) |
| **CON-6** | All student and lecturer information must be stored within the institution’s secure environment or cloud tenant, with encryption at rest (AES-256) and in transit (TLS 1.2+). | Ensures compliance with institutional privacy and security policies. | QA-5 (Security), QA-2 (Availability) |
| **CON-7** | The AI system shall not train on student data outside the institution’s environment. | Protects privacy and prevents data misuse by external vendors. | QA-10 (Data Integrity), QA-5 (Security) |
| **CON-8** | All integrations (LMS, SIS, Calendar) must use official APIs provided by the institution. | Ensures safe, consistent, and maintainable data access. | QA-9 (Interoperability), QA-8 (Modifiability) |
| **CON-9** | Maintain 99% system uptime per month with automatic failover and daily backups (PRO ≤ 15 min, RTO ≤ 1 hour). | Provides high availability and resilience against downtime. | QA-2 (Availability), QA-7 (Resilience) |
| **CON-10** | Answers involving grades, financials, or policies must cite a source link from an approved system. | Prevents misinformation and maintains data trustworthiness. | QA-4 (Usability), QA-10 (Data Integrity) |
