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
