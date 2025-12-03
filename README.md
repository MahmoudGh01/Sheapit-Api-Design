# Sheapit – Logistics app architecture design

Sheapit is a logistics coordination platform connecting shippers, transport companies, and drivers to manage the full shipment lifecycle—from request creation to delivery and invoicing.

This repository contains the architectural design package for the academic project **“Designing Apps and APIs.”**

---

## 🚀 Overview

Sheapit centralizes communication and operations for road-transport logistics.  
It provides multiple frontends, modular backend services, and integrations required to support real-time shipment coordination.

---

## 🧑‍🤝‍🧑 User Types

- **Shipper** – Creates shipment requests, tracks deliveries, reviews invoices.
- **Transport Company (Dispatcher/Manager)** – Assigns shipments, manages fleet, monitors operations.
- **Driver** – Receives tasks on mobile app, updates statuses, provides proof of delivery.

---

## 🖥️ Frontend Applications

- **Shipper Web Dashboard** – Shipment creation & tracking.
- **Transport Company Operations Console** – Planning board, fleet view.
- **Driver Mobile App** – Assignments, live status, proof of delivery.

---

## 🧱 Backend Services

- **Auth & Identity Service**
- **Shipment Management Service**
- **Fleet & Assignment Service**
- **Billing & Invoicing Service**
- **Notifications & Async Jobs Service**

Each service is designed to be independently scalable and accessed through an API Gateway.

---

## 🔑 Core Features

- Create, assign, and track shipments.
- Real-time driver updates + GPS/location tracking.
- Proof of delivery (signature, photos).
- Route + ETA calculation via external Maps API.
- Automated invoicing and payment handling.
- Multi-tenant access control (organizations & roles).

---

## 🗄️ Data Store

Primary storage uses a relational database containing entities such as:

- **Users & Organizations**
- **Drivers & Vehicles**
- **Shipments & Stops**
- **Assignments**
- **Invoices & Payments**

Designed with clear relationships to support reporting, authorization, and operational workflows.