# Software Development Lifecycle (SDLC) Template

> This template is aligned with the standard 7-Phase SDLC process.

    ## Project Information

        **Project Name:** [Project Name]  
        **Project Type:** [Web Application / Mobile App / Desktop Software / etc.]  
        **Project Owner:** [Name/Department]  
        **Project Manager:** [Name]  
        **Version:** [X.X]  
        **Last Updated:** [Date]

---

## Phase 1: Requirements Gathering & Analysis (The "Blueprints")

  ### 1.1 Project Overview
    **Business Problem:**  
    [Describe the problem this project solves]

    **Project Objectives:**
    - Objective = “Why are we doing this project?”
    - [Objective 1] 
    - [Objective 2]

    **Success Metrics (KPIs):**
    - Success Metric = “How do we prove it worked?”
    - [Metric 1: Target value]

  ### 1.2 User Analysis
    **Target Audience:**
    - [Primary User]
    - [Secondary User]

    **User Stories (Format: As a... I want... So that...):**
    ```
    Story ID: US-001
    Title: [Brief title]

    As a [user role],
    I want to [action/feature],
    So that [benefit/value].

    Acceptance Criteria:
    - [ ] [Criterion 1]
    - [ ] [Criterion 2]
    ```

  ### 1.3 Requirements List (MoSCoW Method)
    | ID | Requirement | Priority (Must/Should/Could) |
    |----|-------------|------------------------------|
    | REQ-001 | [e.g. User Login] | Must Have |
    | REQ-002 | [e.g. Dark Mode] | Could Have |

**Phase 1 Deliverables:**
- [ ] Software Requirements Specification (SRS)
- [ ] Requirements Traceability Matrix
- [ ] Risk Assessment

---

## Phase 2: System Design & Architecture (The "Architect")

  ### 2.1 Architecture Decision Records (ADR)
    **Architecture Pattern:** [Monolithic / Microservices / Serverless]
    
    **Reasoning:**
    [Why did we choose this pattern? e.g. "Chosen Microservices for scalability"]

  ### 2.2 Database Design
    **Database Type:** [PostgreSQL / MongoDB / Firebase / etc.]

    **Schema / ER Diagram:**
    [Link to ER Diagram or text description]

  ### 2.3 API Design (Contract First)
    ```
      Endpoint: GET /api/v1/users
      Description: Fetch user profile
      Response: { "id": 1, "name": "John" }
    ```

  ### 2.4 UI/UX Design
    **Design System:** [Material UI / Tailwind / Custom]
    **Mockups:** [Link to Figma/Adobe XD]

**Phase 2 Deliverables:**
- [ ] Architecture Diagram
- [ ] Database Schema
- [ ] API Specification (Swagger/OpenAPI)
- [ ] Security Design Document

---

## Phase 3: Implementation (Coding) (The "Construction")

  ### 3.1 Technology Stack
    - **Frontend:** [React / Vue / Angular]
    - **Backend:** [Node.js / Python / Go]
    - **Database:** [PostgreSQL / MongoDB]
    - **Tools:** [Docker / Kubernetes]

  ### 3.2 Development Standards
    - **Code Style:** [e.g. PEP8 for Python, Airbnb for JS]
    - **Branching Strategy:** [GitFlow / Trunk-Based]
    
  ### 3.3 Tasks & Milestones
    | Sprint | Feature Set | Owner | Due Date |
    |--------|-------------|-------|----------|
    | Sprint 1 | Authentication | [Name] | [Date] |
    | Sprint 2 | Dashboard | [Name] | [Date] |

**Phase 3 Deliverables:**
- [ ] Source Code (GitHub/GitLab)
- [ ] Code Review Sign-offs
- [ ] Unit Tests written

---

## Phase 4: Testing (The "Inspection")

  ### 4.1 Testing Strategy (The Pyramid)
    - **Unit Tests (70%):** [Jest / Pytest]
    - **Integration Tests (20%):** [Supertest / Postman]
    - **E2E Tests (10%):** [Cypress / Selenium]

  ### 4.2 Test Plan
    | ID | Test Case | Functionality | Status |
    |----|-----------|---------------|--------|
    | TC-001 | Login with valid credentials | Auth | Pass |
    | TC-002 | Login with invalid password | Auth | Pass |

**Phase 4 Deliverables:**
- [ ] Test Execution Report (Pass/Fail)
- [ ] Performance Test Results (Load Testing)
- [ ] Security Audit Report

---

## Phase 5: Deployment (The "Move-In")

  ### 5.1 Deployment Strategy
    **Strategy:** [Blue-Green / Canary / Rolling]
    
  ### 5.2 CI/CD Pipeline
    **Triggers:** Push to `main` branch
    **Steps:**
    1. Lint & Format check
    2. Run Unit Tests
    3. Build Docker Image
    4. Deploy to Staging
    5. Manual Approval -> Deploy to Production

  ### 5.3 Rollback Plan
    **If failure occurs:**
    1. Revert Git commit
    2. Run `rollback.sh` script
    3. Restore database backup (if needed)

**Phase 5 Deliverables:**
- [ ] Deployment Runbook
- [ ] Release Notes (v1.0.0)
- [ ] Monitoring Dashboard URLs

---

## Phase 6: Maintenance & Operations (The "Upkeep")

  ### 6.1 Monitoring (The 4 Golden Signals)
    - **Latency:** [Target: < 200ms]
    - **Traffic:** [Target: 1000 req/sec]
    - **Errors:** [Target: < 1% error rate]
    - **Saturation:** [Target: < 70% CPU usage]

  ### 6.2 Incident Response (On-Call)
    **Severity Levels:**
    - **SEV-1 (Critical):** Site down. Immediate response (PagerDuty).
    - **SEV-2 (High):** Feature broken. Fix within 24h.
    - **SEV-3 (Low):** Minor bug. Fix next sprint.

  ### 6.3 Post-Mortem Template
    *Use this if a major incident occurs.*
    - **What happened?**
    - **Root Cause?**
    - **Resolution?**
    - **Action Items?**

---

## Phase 7: Iteration & Evolution (The "Remodel")

  ### 7.1 Feedback Loop
    **Sources:**
    - User Analytics (Mixpanel/Google Analytics)
    - App Store Reviews
    - Customer Support Tickets

  ### 7.2 Technical Debt Log
    | Item | Impact | Complexity | Plan to Fix |
    |------|--------|------------|-------------|
    | [e.g. Old API version] | Low | High | Q3 Refactor |

  ### 7.3 Next Steps (Roadmap)
    - [ ] Feature A for v2.0
    - [ ] Performance Optimization
    
---

## Sign-offs

| Phase | Stakeholder | Date | Signature |
|-------|-------------|------|-----------|
| Requirements | | | |
| Design | | | |
| Launch Approval | | | |

---
**Template Version:** 2.0 (Aligned with SDLC Guide)
