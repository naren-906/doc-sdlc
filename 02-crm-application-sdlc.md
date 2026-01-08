# Software Development Lifecycle (SDLC) - CRM: Contact Management Module

> This document is a **filled example** of the `01-sample-template.md`, focusing on the "Contact Management" feature of a CRM.

    ## Project Information

        **Project Name:** CRM System - Module: Smart Contact Manager  
        **Project Type:** SaaS Web Application  
        **Project Owner:** Sales Operations Dept  
        **Project Manager:** Narenthiran  
        **Version:** 1.0.0-beta  
        **Last Updated:** 2026-01-08

---

## Phase 1: Requirements Gathering & Analysis (The "Blueprints")

  ### 1.1 Project Overview
    **Business Problem:**  
    Sales representatives currently use Excel sheets to manage client data. This leads to data duplication, loss of contact history, and inability to share leads across the team.

    **Project Objectives:**
    - Objective = “Why are we doing this project?”
    - Centralize all customer data into a single, searchable database.
    - Enable sharing of contact history between sales reps.
    - Automate data entry from email signatures.

    **Success Metrics (KPIs):**
    - Success Metric = “How do we prove it worked?”
    - Reduce time spent searching for phone numbers by 50%.
    - Achieve 100% data accuracy (no duplicate contacts).
    - User Adoption: 90% of sales team uses it daily within 3 months.

  ### 1.2 User Analysis
    **Target Audience:**
    - **Primary User:** Sales Representatives (Daily usage for calls/emails).
    - **Secondary User:** Sales Managers (Weekly usage for reporting).

    **User Stories:**
    ```
    Story ID: US-001
    Title: Create New Contact

    As a Sales Representative,
    I want to manually add a new contact with name, email, and phone,
    So that I can save prospect details after a networking event.

    Acceptance Criteria:
    - [ ] System verifies email format (valid@domain.com).
    - [ ] System checks for duplicates before saving.
    - [ ] 'Save' button is disabled until mandatory fields are filled.
    ```

  ### 1.3 Requirements List (MoSCoW Method)
    | ID | Requirement | Priority |
    |----|-------------|----------|
    | REQ-001 | Create/Read/Update/Delete (CRUD) Contact | Must Have |
    | REQ-002 | Search contacts by Name or Company | Must Have |
    | REQ-003 | Import contacts from CSV | Should Have |
    | REQ-004 | "Scan Business Card" feature | Could Have |

**Phase 1 Deliverables:**
- [x] Software Requirements Specification (SRS) - *Signed off*
- [x] Requirements Traceability Matrix
- [x] Risk Assessment (Data privacy risk identified)

---

## Phase 2: System Design & Architecture (The "Architect")

  ### 2.1 Architecture Decision Records (ADR)
    **Architecture Pattern:** Microservices
    
    **Reasoning:**
    The Contact module needs to scale independently from the Reporting module. If Reporting crashes (heavy load), Contacts (light load) must still work so Sales Reps can make calls.

  ### 2.2 Database Design
    **Database Type:** PostgreSQL (Relational)

    **Schema:**
    - Table: `contacts`
        - `id` (UUID, PK)
        - `first_name` (VARCHAR)
        - `email` (VARCHAR, UNIQUE)
        - `company_id` (FK)
        - `created_at` (TIMESTAMP)

  ### 2.3 API Design (Contract First)
    ```
      Endpoint: POST /api/v1/contacts
      Description: Create a new contact
      Payload: { "name": "Alice", "email": "alice@corp.com" }
      Response: 201 Created { "id": "uuid-1234" }
    ```

  ### 2.4 UI/UX Design
    **Design System:** Material UI (React)
    **Mockups:** [Figma Link: Contact List View]

**Phase 2 Deliverables:**
- [x] Architecture Diagram
- [x] Database Schema v1.0
- [x] API Swagger Spec

---

## Phase 3: Implementation (Coding) (The "Construction")

  ### 3.1 Technology Stack
    - **Frontend:** React.js (TypeScript)
    - **Backend:** Node.js (NestJS)
    - **Database:** PostgreSQL 15
    - **Tools:** Docker, Github Actions

  ### 3.2 Development Standards
    - **Code Style:** ESLint "Standard" config + Prettier
    - **Branching Strategy:** GitHub Flow (Feature branches merge to Main)
    
  ### 3.3 Tasks & Milestones
    | Sprint | Feature Set | Owner | Due Date |
    |--------|-------------|-------|----------|
    | Sprint 1 | API Setup + Database Migration | Backend Team | Jan 15 |
    | Sprint 2 | Contact List UI + Search | Frontend Team | Jan 22 |
    | Sprint 3 | Edit/Delete Logic + Integration | Full Stack | Jan 29 |

**Phase 3 Deliverables:**
- [ ] Source Code (repo: `crm-contacts-service`)
- [ ] Pull Requests merged
- [ ] 80% Unit Test Coverage

---

## Phase 4: Testing (The "Inspection")

  ### 4.1 Testing Strategy
    - **Unit Tests (Jest):** Test email validation logic isolated from DB.
    - **Integration Tests (Supertest):** Test POST /contacts actually writes to Test DB.
    - **E2E Tests (Cypress):** Test user logging in and clicking "Add Contact".

  ### 4.2 Test Plan
    | ID | Test Case | Functionality | Status |
    |----|-----------|---------------|--------|
    | TC-001 | Add valid contact | CRUD | Pass |
    | TC-002 | Add duplicate email | Validation | Pass |
    | TC-003 | Delete contact | CRUD | Pass |

**Phase 4 Deliverables:**
- [ ] Test Run Report (Green Build)
- [ ] Load Test (Verify search speed with 10k contacts)

---

## Phase 5: Deployment (The "Move-In")

  ### 5.1 Deployment Strategy
    **Strategy:** Rolling Deployment
    *Update one server instance at a time to ensure zero downtime for Sales Reps.*
    
  ### 5.2 CI/CD Pipeline
    **Triggers:** Push to `main`
    **Steps:**
    1.  `npm install`
    2.  `npm run lint` & `npm test`
    3.  Build Docker Image: `crm-contacts:v1.0`
    4.  Push to AWS ECR
    5.  Update Kubernetes Deployment (Staging) -> (Production)

  ### 5.3 Rollback Plan
    **If failure:** `kubectl undo deployment/crm-contacts` (Reverts to previous image immediately).

**Phase 5 Deliverables:**
- [ ] Deployment Verified in Production
- [ ] Release Notes sent to Sales Team

---

## Phase 6: Maintenance & Operations (The "Upkeep")

  ### 6.1 Monitoring (The 4 Golden Signals)
    - **Latency:** API response < 200ms
    - **Errors:** 5xx responses < 0.1%
    - **Traffic:** Track number of `POST /contacts` per day
    - **Saturation:** Database connections < 80% max

  ### 6.2 Incident Response
    - **SEV-1:** Cannot view contacts aka "Sales Global Outage". (Wake up DevOps).
    - **SEV-3:** "Sort by Name" is buggy. (Ticket for next sprint).

**Phase 6 Deliverables:**
- [ ] Datadog Dashboard Link
- [ ] PagerDuty Schedule Clean

---

## Phase 7: Iteration & Evolution (The "Remodel")

  ### 7.1 Feedback Loop
    - Sales Rep Feedback: "Adding contacts takes too many clicks."
    - Action: Plan "Quick Add" widget for v1.1.

  ### 7.2 Technical Debt Log
    | Item | Impact | Complexity | Plan to Fix |
    |------|--------|------------|-------------|
    | Hardcoded error messages | Low | Low | Next Sprint |

  ### 7.3 Next Steps (Roadmap)
    - [ ] v1.1: Outlook/Gmail Integration
    - [ ] v1.2: AI-based "Lead Scoring" for contacts

---

## Sign-offs

| Phase | Stakeholder | Date | Signature |
|-------|-------------|------|-----------|
| Requirements | Head of Sales | 2026-01-02 | *Approve* |
| Design | Tech Lead | 2026-01-05 | *Approve* |
| Launch Approval | CTO | | |

---
**Status:** In Development
