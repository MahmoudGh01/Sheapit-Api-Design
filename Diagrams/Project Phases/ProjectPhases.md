# 📦 Sheapit – Project Phases Documentation

## Overview

Sheapit is a logistics platform connecting **Shippers**, **Transport Companies**, and **Drivers** through mobile apps and a web dashboard.  
The system is built using a **microservices architecture** and evolves in clearly defined phases.

---

## 🧩 MVP Architecture (Minimum Viable Product)

### Goal

Validate the core logistics workflow:
- Create transport requests
- Receive offers
- Accept offers
- Execute shipments
- Track deliveries

### Actors
- **Shipper**
- **Transport Company**
- **Driver**
- **Admin**

### Client Applications
- **Shipper Mobile App**
- **Driver Mobile App**
- **Web Dashboard** (Admin & Transport Company)

### Backend Services
- **API Gateway**
- **Auth Service**
- **Users Service**
- **Transport Request Service**
- **Offers Service**
- **Shipment Service**
- **Drivers Service**

### Infrastructure
- Dedicated database per service
- Object storage (S3) for documents
- Basic real-time tracking

### MVP Characteristics
- Synchronous REST communication
- Authentication via JWT
- No advanced automation
- No payments
- Minimal monitoring

---

## 🚀 Phase I – Release-Ready Architecture

### Goal

Make the system production-ready and usable by real customers.

### Added Components (on top of MVP)

#### Security & Stability
- JWT validation at API Gateway
- Role-Based Access Control (RBAC)
- Rate limiting

#### Communication
- Notification Service (email / push)
- Event Bus (async communication)

#### Operations
- Centralized logging
- Metrics & monitoring
- Error tracking

#### Business Logic
- Pricing / fees service
- Manual or placeholder payment flow

### Phase I Outcome
- Reliable user experience
- Decoupled services
- Observable system
- Ready for real usage

---

## 🧠 Phase II – Optimization & Intelligence

### Focus

Automation, scalability, and monetization.

### Planned Additions
- Smart matching engine (shipments ↔ drivers)
- Dynamic pricing
- Online payments & invoicing
- Advanced real-time GPS tracking
- Analytics & performance dashboards

---

## 🔮 Future Phases

- Multi-country & multi-currency support
- Public partner API
- AI-based route optimization
- Demand forecasting
- Carbon footprint tracking

---

## 📌 Implementation Strategy

### Priority Order
1. User pain points (missed notifications, delays)
2. Business blockers (manual pricing, no payments)
3. Operational risks (lack of logs, failures)

### Decision Signals
- High manual workload → automation
- Pricing disputes → pricing engine
- Delivery delays → tracking & ETA prediction