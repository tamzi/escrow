## 🏗 1. High-Level Architecture
escrow follows a microservices approach. 
Escrow has smaller, loosely coupled services that communicate via REST APIs and event-driven messaging.

### Architecture Overview
Each microservice will handle a specific domain responsibility and can be deployed independently.

### 🛠 Core Microservices
1️⃣ User Service → Handles authentication, user profiles, and role-based access control (RBAC).
2️⃣ Escrow Service → Manages escrow transactions, state changes, and holds funds temporarily.
3️⃣ Payment Service → Integrates with Stripe/PayPal to process deposits and releases.
4️⃣ Notification Service → Sends emails, SMS, and app notifications on transaction updates.
5️⃣ Dispute Service → Manages buyer-seller conflicts and admin intervention.
6️⃣ API Gateway → Acts as a single entry point for all services.
7️⃣ Logging & Monitoring → Observability tools for tracking system health.

### 🔗 Inter-Service Communication
- REST APIs for synchronous communication
- Message Queues (Kafka/RabbitMQ) for asynchronous communication
- Webhooks for event-driven updates


## 🏛 2. Key Architectural Decisions
1️⃣ Microservices


2️⃣ Authentication & Security
Spring Security + JWT/OAuth2
Role-based access:
BUYER → Can create & fund transactions
SELLER → Can fulfill transactions
ADMIN → Can resolve disputes


3️⃣ Database & Persistence
PostgreSQL (Prod) | H2 (Dev)
Entities:
Users (Buyers, Sellers, Admins)
Escrow Transactions (status: PENDING, FUNDED, RELEASED, DISPUTED)
Audit Logs


4️⃣ Payments
Integrate with Stripe/PayPal
Webhook-based confirmation system


5️⃣ Notifications & Messaging
Emails (SendGrid)
SMS (Twilio)
WebSockets (Real-time updates for UI)


6️⃣ Logging & Monitoring
ELK Stack (Elasticsearch, Logstash, Kibana)
Prometheus + Grafana (Metrics Monitoring)




### 📚 2. Technology stack


## 📦 3. Modules

#### 1️⃣ escrow-core
#### 1️⃣ escrow-api
##### 📂 1.1.1. Features

- **Authentication & Authorization**
  - Register, login, and update user profiles
  - Role-based access control (RBAC) for buyers, sellers, and admins
  - JWT token generation and validation


## 🏗 4. Architecture Diagram
      
                            ┌────────────────────────────────────────────┐
                            │           Client (Web / Mobile)            │
                            └────────────────────────────────────────────┘
                                                   │
                                ┌──────────────────▼───────────────────┐
                                │       API Gateway (Spring Cloud)     │
                                │       - Routing, Security            │
                                │       - Rate Limiting, CORS          │
                                └──────────────────────────────────────┘
                                                   │
                ┌──────────────────────────────────┼──────────────────────────────────┐
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         User Service (Spring Boot, Kotlin)                       ││
                │ │ - JWT Authentication, RBAC                                       ││
                │ │ - User Registration & Profile Mgmt                               ││
                │ └──────────────────────────────────────────────────────────────────┘│
                │                                  │                                  │
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         Escrow Service (Spring Boot, Kotlin)                     ││
                │ │ - Creates escrow transactions                                    ││
                │ │ - Manages escrow states (Pending, Funded, Released)              ││
                │ └──────────────────────────────────────────────────────────────────┘│
                │                                  │                                  │
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         Payment Service (Spring Boot, Kotlin)                    ││
                │ │ - Stripe / PayPal API Integration                                ││
                │ │ - Handles fund deposits & releases                               ││
                │ └──────────────────────────────────────────────────────────────────┘│
                │                                  │                                  │
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         Notification Service (Spring Boot, Kotlin)               ││
                │ │ - Sends Emails, SMS (SendGrid, Twilio)                           ││
                │ └──────────────────────────────────────────────────────────────────┘│
                │                                  │                                  │
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         Dispute Service (Spring Boot, Kotlin)                    ││
                │ │ - Handles disputes & admin intervention                          ││
                │ └──────────────────────────────────────────────────────────────────┘│
                │                                  │                                  │
                │                                  ▼                                  │
                │ ┌──────────────────────────────────────────────────────────────────┐│
                │ │         Logging & Monitoring (ELK, Prometheus, Grafana)          ││
                │ │ - Centralized Logging & Alerts                                   ││
                │ └──────────────────────────────────────────────────────────────────┘│


## 🚧 To be done
- 