# Sheapit – Logistics App Architecture Design

Sheapit is a logistics coordination platform connecting shippers, transport companies, and drivers throughout the shipment lifecycle—from creation to delivery and invoicing.  
This repository contains the architectural design package for the academic project **“Designing Apps and APIs.”**

---

## 🚀 Overview

Sheapit centralizes road-transport operations with multiple frontends, modular backend services, and integrations for real-time coordination.

### 🔑 Core Features
- Create, assign, and track shipments
- Real-time driver location updates
- Proof of delivery (photos, signatures)
- Route + ETA via external Maps API
- Automated invoicing
- Multi-tenant RBAC access control

---

## 🧑‍🤝‍🧑 User Types
- **Shipper** – creates shipments, tracks delivery, views invoices
- **Transport Company** – dispatches shipments, manages drivers/vehicles
- **Driver** – receives assignments, updates status, sends POD

---

## 🗄️ Data Store

A relational database (PostgreSQL) storing:
- Users & Organizations
- Drivers & Vehicles
- Shipments & Stops
- Assignments
- Invoices & Payments

Designed for integrity, reporting, and authorization use cases.

---

## 🚀 Project Elements

### Frontends
- **Shipper Web Dashboard**
- **Transport Console**
- **Driver Mobile App**

### Backend Services
- Auth & Identity
- Shipment Management
- Fleet & Assignment
- Billing & Invoicing
- Maps/Route Service
- Media/File Service
- Notifications & Async Jobs

### Infrastructure
- PostgreSQL (primary DB)
- Redis (cache for GPS, ETAs, active shipments)
- S3 (proof-of-delivery storage)
- Message Broker (Kafka/SQS/RabbitMQ)
- API Gateway
- Monitoring & Logging (CloudWatch / ELK)

---

## 🧱 Architecture Patterns

- **Microservices behind an API Gateway**
- **Admin Monolith** for back-office operations (organizations, roles, audits)

### Microservices
- Authentication
- Shipment
- Fleet & Assignment
- Billing
- Maps/Route
- Media Storage

### Serverless / Async Workers
- Notifications (email, SMS, push)
- ETA recalculations
- POD processing
- Invoice generation

### Message Queue Events
- `ShipmentCreated`
- `ShipmentAssigned`
- `StatusUpdated`
- `ProofOfDeliveryCaptured`
- `InvoiceIssued`

---

## 🔌 Communication Types

### Synchronous (REST/gRPC)
- Frontend → Gateway → Services
- Standard CRUD operations
- Calls to external Maps API

### Asynchronous (Events / Queues)
- Shipment & assignment lifecycle events
- Notification dispatch
- Billing workflows
- Background jobs

### Real-Time (WebSockets)
- Live shipment and driver location updates
- Dynamic transport console dashboards

---

## 🔐 AuthN & AuthZ

### Authentication
- JWT access tokens (web + mobile)
- OAuth2/OIDC login
- Refresh tokens for session renewal

### Authorization
- Role-based: Shipper, Dispatcher, Driver, Accountant, Admin
- API Gateway: token validation + scopes
- Services: enforcement of business rules per organization  

flowchart TB

%% ============================
%%          STYLING
%% ============================
classDef user fill:#ffcc80,stroke:#e57c00,stroke-width:1px,color:#000
classDef client fill:#ffe6cc,stroke:#cc6600,stroke-width:1px,color:#000
classDef internal fill:#cce0ff,stroke:#3366cc,stroke-width:1px,color:#000
classDef db fill:#d9d9d9,stroke:#666,stroke-width:1px,color:#000
classDef external fill:#ccffcc,stroke:#339933,stroke-width:1px,color:#000
classDef boundary stroke-dasharray: 5 5

%% ============================
%%          USERS
%% ============================
subgraph Users
U1(Shipper)
U2(Transport Company)
U3(Driver)
end
class Users boundary
class U1,U2,U3 user

%% ============================
%%      CLIENT APPLICATIONS
%% ============================
subgraph Client_Apps[Client Applications (Public)]
WA(Web App - Shipper Portal<br/>Next.js)
MA1(Mobile App - Shipper/Requester<br/>React Native)
MA2(Mobile App - Driver/Carrier<br/>React Native)
end
class Client_Apps boundary
class WA,MA1,MA2 client

%% Connections: Users → Apps
U1 -->|Uses HTTPS + JWT| WA
U1 -->|Uses HTTPS + JWT| MA1
U2 -->|Uses HTTPS + JWT| MA2
U3 -->|Uses HTTPS + JWT| MA2

%% ============================
%%            BACKEND
%% ============================
subgraph Backend[Backend – Modular Monolith API (Public API Layer)]
API(Auth Module<br/>JWT Validation)
REQ(Requests Module)
OFF(Offers Module)
SHP(Shipments Module<br/>State Machine)
RT(Realtime Module<br/>WebSockets Provider)
end
class Backend boundary
class API,REQ,OFF,SHP,RT internal

%% Connections: Apps → Backend
WA -->|HTTPS REST| API
MA1 -->|HTTPS REST| API
MA2 -->|HTTPS REST / WebSockets| API
API --> REQ
API --> OFF
API --> SHP
SHP --> RT

%% ============================
%%      INTERNAL COMPONENTS
%% ============================
subgraph Internal_Systems[Internal Only]
DB[(PostgreSQL Database)]
JOBS(Background Jobs<br/>Notifications, Lifecycle)
end
class Internal_Systems boundary
class DB db
class JOBS internal

%% Connections: Backend → Internal
REQ --> DB
OFF --> DB
SHP --> DB
RT --> DB
SHP --> JOBS
JOBS --> DB

%% ============================
%%        EXTERNAL SERVICES
%% ============================
subgraph External_Providers[External Providers (Public → Backend Only)]
AUTH(Auth Provider<br/>OAuth2 / OIDC)
WS(WebSockets Provider<br/>Pusher/Ably)
NOTIF(Notifications<br/>Email/SMS/Push)
end
class External_Providers boundary
class AUTH,WS,NOTIF external

%% Connections: Backend → External
API -->|OAuth2 Login| AUTH
RT -->|WebSockets| WS
JOBS -->|Send Messages| NOTIF
