# Project Specifications Document (Cahier des Charges)

**Project Deadline:** May 29th

## 1. Project Overview
The goal of this project is to build a highly secure, microservices-based inventory management system ("Scorpion"). The system requires strict access control, inventory tracking, need-based stock lists, and operation resource analytics.

## 2. Architecture & Technology Stack
- **Architecture:** Microservices
- **Security Standard:** Zero-Trust / Default Deny (everything is forbidden by default)
- **Database:** PostgreSQL (with Prisma ORM for specific services)
- **Primary Language:** TypeScript (for Auth service, potentially others)
- **API Style:** REST API

## 3. Microservices Breakdown

### 3.1. Frontend Service ("Hermes")
- **Authentication Handling:** Integrates with Discord Auth. Stores the ID/cookie required by the Auth service.
- **Internationalization (i18n):** The frontend must be built with translation in mind, making it easy to add and switch languages.
- **UI/UX:** Includes a main Dashboard.

### 3.2. Authentication Service ("wan-shi-tong")
*Note: Consider evaluating pre-made open-source auth services (e.g., Keycloak, Authelia, Logto) before building from scratch.*
- **Tech Stack:** TypeScript, REST API, PostgreSQL with Prisma.
- **Features:** 
  - Manage user sessions.
  - Configurable session length via API.
  - Centralized authority for verifying user rights.

### 3.3. Inventory Service ("Renenutet")
- **Tech Stack:** PostgreSQL.
- **Features:** 
  - CRUD operations for inventory items.
  - Interacts with the Auth service (`wan-shi-tong`) to verify permissions before executing operations.

### 3.4. Knowledge Base Service ("Krang")
- **Tech Stack:** TypeScript, PostgreSQL with Prisma.
- **Features:**
  - Automated web scraper to extract and sync data from a wiki.
  - Stores queried database of knowledge for the rest of the application ecosystem.

## 4. Functional Requirements

### 4.1. Inventory Management
- General stock tracking (addition, modification, deletion).
- **Target Stock Levels:** Ability to specify how much of an item is needed. UI must clearly highlight these deficits.

### 4.2. Need Lists
- **Standard Needed List:** Items that are below target stock but not immediately critical.
- **Urgent Needed List:** Critical items that must be displayed first/prominently on the dashboard.

### 4.3. Analytics & Tracking
- Track consumption and production of resources over specific time frames.
- **Goal:** Evaluate how many resources a specific operation/event consumes or produces.

## 5. Roles & Permissions (RBAC)
- **Offi (Officer) +**: Can modify the *needed* stock levels.
- **Member +**: Can modify the *current* stock quantities.
- **Logisticians**: Can see "the codes" (specific sensitive logistical data/access).

## 6. Infrastructure & Operations (Non-Functional Requirements)
- **Security First:** The overarching principle for all development.
- **Logs & Audit:** Comprehensive logging system for all actions.
- **Backups & Recovery:** 
  - Automated database backups.
  - Ability to reconstruct the database state from the event/action logs (Event Sourcing or detailed audit trails).

---

## 7. Suggested Task Breakdown (For the Team)

To help you and your friends get started, here is a suggested way to distribute the work:

### Task Force 1: Frontend ("Hermes")
- [ ] Set up the frontend framework and routing.
- [ ] Implement Discord OAuth2 integration.
- [ ] Set up the i18n (translation) library.
- [ ] Build the Main Dashboard, Urgent List, and Needed List views.
- [ ] Build the Inventory modification UI.

### Task Force 2: Authentication ("wan-shi-tong")
- [ ] Decide on custom vs. pre-made auth service.
- [ ] Set up the project (TypeScript, Prisma, Postgres).
- [ ] Build login/Discord validation endpoints.
- [ ] Build endpoints for session length configuration.
- [ ] Implement middleware/endpoints for verifying roles (Member, Offi, Logistician).

### Task Force 3: Inventory DB & API ("Renenutet")
- [ ] Design the database schema (Items, Transactions/Logs, Target Stocks).
- [ ] Implement REST endpoints for CRUD operations.
- [ ] Implement the logic to connect to `wan-shi-tong` for permission validation on every route.
- [ ] Build the consumption/production historical tracking logic.

### Task Force 4: Knowledge Base & Scraper ("Krang")
- [ ] Set up the Krang project (TypeScript, Prisma, Postgres).
- [ ] Design the database schema for the wiki data.
- [ ] Build the associated web scraper to fetch relevant data from the target wiki.
- [ ] Implement a periodic sync task and an API to serve this data.

### Task Force 5: DevOps & Infrastructure
- [ ] Set up Docker / Docker Compose for the microservices.
- [ ] Implement the logging architecture.
- [ ] Implement database backup scripts.
- [ ] Research/implement the database reconstruction from logs feature.
