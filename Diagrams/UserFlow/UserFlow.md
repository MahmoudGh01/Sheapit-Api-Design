# 📦 Sheapit — Core User Flows & Sequence Diagrams

## 1. System Overview

Sheapit is a logistics coordination platform that connects **Shippers**, **Transport Companies**, and **Drivers** through mobile and web applications.  
The system follows a **microservices architecture** with an API Gateway, authentication, and domain-specific services.

This document describes:
- Core user flows
- Unique system behaviors
- Key sequence diagrams for critical flows

---

## 2. Core User Flows (6–10)

### 1. User Sign In (JWT Authentication)

#### Elements Involved
- **Mobile App / Web App**
- **API Gateway**
- **Auth Service**
- **User Database**
- **JWT / Refresh Token Service**

#### Notes
- Standard authentication flow
- Uses JWT + refresh tokens

---

### 2. Create & Publish Transport Request

#### Elements Involved
- **Shipper App**
- **API Gateway**
- **Transport Request Service**
- **Database**
- **Notification Service**

#### Why It Matters
- Entry point of the logistics workflow

---

### 3. View Transport Requests (Transport Company)

#### Elements Involved
- **Web Dashboard**
- **API Gateway**
- **Transport Request Service**
- **Database**

---

### 4. Submit Offer for a Transport Request

#### Elements Involved
- **Transport Company App**
- **API Gateway**
- **Offer Service**
- **Database**

---

### 5. View Offers for a Transport Request

#### Elements Involved
- **Shipper App**
- **API Gateway**
- **Offer Service**
- **Database**

---

### 6. Accept Offer & Create Shipment ✅ (Unique)

#### Elements Involved
- **Shipper App**
- **API Gateway**
- **Offer Service**
- **Shipment Service**
- **Database**
- **Notification Service**

#### Why Unique
- Atomic operation:
    - Locks transport request
    - Accepts exactly one offer
    - Creates shipment

---

### 7. Assign Driver to Shipment ✅ (Unique)

#### Elements Involved
- **Transport Company App**
- **API Gateway**
- **Shipment Service**
- **Driver Service**
- **Database**
- **Notification Service**

#### Why Unique
- Cross-service validation (shipment + driver)
- Strong RBAC enforcement

---

### 8. Track Shipment (Basic Real-Time)

#### Elements Involved
- **Driver App**
- **Shipment Service**
- **Tracking Component**