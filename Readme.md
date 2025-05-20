## 🧩 **Microservice Architecture – AI-CRM**

The AI-CRM platform follows a modular, scalable **microservice architecture** to support independent feature deployment, seamless third-party integrations, and intelligent data processing.

---

### 📌 **High-Level Architecture Diagram**

```
                                        ┌──────────────────────┐
                                        │     Web Frontend     │
                                        │     (Next.js App)    │
                                        └─────────┬────────────┘
                                                  │
                                       API Gateway / BFF Layer
                                                  │
────────────────────────────────────────────────────────────────────────────────────
                                                  ▼
 ┌──────────────────┐     ┌────────────────┐     ┌──────────────────────┐
 │ Auth Service     │     │ User Service   │     │ Role/Permission Svc  │
 │ (JWT, OAuth)     │     │ (Teams, Users) │     │ RBAC Management      │
 └──────────────────┘     └────────────────┘     └──────────────────────┘

 ┌─────────────────────────────┐
 │ Contact Management Service  │ ────────┐
 └─────────────────────────────┘        │
 ┌─────────────────────────────┐        │
 │ Company Management Service  │        │
 └─────────────────────────────┘        │
 ┌─────────────────────────────┐        ▼
 │ Job & Candidate Service     │ <── Resume Classifier Service (AI)
 └─────────────────────────────┘        │
 ┌─────────────────────────────┐        ▼
 │ Project & Task Service      │        │
 └─────────────────────────────┘        │
 ┌─────────────────────────────┐        ▼
 │ Idea Ingestion Service      │ <── Typeform Middleware Service
 └─────────────────────────────┘        │
 ┌─────────────────────────────┐        ▼
 │ Email Integration Service   │ <── Gmail API + App Script
 └─────────────────────────────┘
 ┌─────────────────────────────┐
 │ AI Search Service (RAG)     │ <── OpenAI Embedding + Vector DB
 └─────────────────────────────┘

────────────────────────────────────────────────────────────────────────────────────

         ┌──────────────────────────────┐        ┌────────────────────────────┐
         │       Shared DB Layer        │◄──────►│ PostgreSQL / Redis / VDB   │
         └──────────────────────────────┘        └────────────────────────────┘

         ┌──────────────────────────────┐
         │  Object Storage Service      │ → AWS S3 (Resumes, Ideas, Attachments)
         └──────────────────────────────┘

         ┌──────────────────────────────┐
         │     DevOps & Observability   │ → GitHub Actions, Prometheus, Grafana, Sentry
         └──────────────────────────────┘
```

---

### 🛠️ **Microservice Breakdown**

| Service Name                 | Responsibility                                                               |
| ---------------------------- | ---------------------------------------------------------------------------- |
| **Auth Service**             | Handles authentication (JWT, session, OAuth), user roles, and token refresh. |
| **User Service**             | Stores user data, teams, and collaborator management.                        |
| **Contact/Company Service**  | CRUD and tracking of contacts and client companies.                          |
| **Job/Candidate Service**    | Job openings, candidate data, resume uploads, and internal notes.            |
| **Resume Classifier**        | AI service to classify resumes into structured data using ML/NLP.            |
| **Idea Ingestion Service**   | Fetches and manages startup ideas from third-party platforms like Typeform.  |
| **Summarization Middleware** | Uses OpenAI to summarize Typeform/Gmail data into actionable insights.       |
| **Email Integration**        | Fetches emails, parses threads, integrates with CRM items.                   |
| **AI Search (RAG)**          | Performs contextual search using vector DB and OpenAI embeddings.            |
| **Project/Task Service**     | Handles internal projects and task workflows.                                |

---

### 🔄 **Communication & Integration**

- **Synchronous (HTTP/REST)**: Between frontend and API gateway/microservices.
- **Asynchronous (Queue/Event Bus)**: For background tasks like summarization, classification, and email syncing (e.g., using Redis Pub/Sub or RabbitMQ).
- **gRPC (Optional)**: For high-performance internal communication between services.

---

### 🌐 **Infra and Deployment**

- **Containerized with Docker**
- **Orchestrated via Kubernetes**
- **CI/CD with GitHub Actions**
- **Secrets and Config Management** with AWS SSM or Vault
- **Monitoring/Logging**: Prometheus, Grafana, Loki, and Sentry
