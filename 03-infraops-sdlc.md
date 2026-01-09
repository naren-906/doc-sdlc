# Software Development Lifecycle (SDLC) Template

> This template is aligned with the standard 7-Phase SDLC process.

    ## Project Information
        **Project Name:** Azhiru InfraOps 
        **Project Type:** Infrastructure Operations Tool
        **Project Owner:** Subbu sainath
        **Project Manager:** Narenthiran
        **Version:** 1.0.0-mvp
        **Last Updated:** 2024-06-10

---

## Phase 1: Requirements Gathering & Analysis (The "Blueprints")

  ### 1.1 Project Overview
    **Business Problem:**
    The Devops and the cloud infrastructure teams face challenges in managing and automating infrastructure operations efficiently and cost optmization. Manual processes lead to errors, delays, and increased operational costs.

    **Project Objectives:**
    - Objective = “Why are we doing this project?”
    - Cost optimization for infra management
    - Centralize infrastructure management tasks into a single platform.
    - Automate routine infrastructure operations to reduce manual effort.
    - Provide real-time monitoring and alerting for infrastructure health.

    **Success Metrics (KPIs):**
    - Success Metric = “How do we prove it worked?”
    - Reduce manual intervention in infra operations by 60%.
    - Achieve 30% cost savings on cloud infrastructure within 6 months.
    - User Adoption: 85% of infra team uses the tool daily within 2 months

  ### 1.2 User Analysis
    **Target Audience:**
    - **Primary User:** DevOps Engineers (Daily usage for infra management).
    - **Secondary User:** Cloud Infrastructure Managers (Weekly usage for reporting).

    **User Stories (Format: As a... I want... So that...):**
    ```
    Story ID: US-001
    Title: Infrastructure Usage Monitoring

    As a DevOps Engineer,
    I want to monitor real-time usage of cloud resources,
    So that I can identify underutilized resources and optimize costs.

    Acceptance Criteria:
    - [ ] Dashboard displays real-time metrics for CPU, memory, and storage usage.
    - [ ] Alerts are triggered for resources exceeding defined thresholds.
    - [ ] Historical usage data is available for trend analysis.
    - [ ] Exportable reports for management review.
    - [ ] User-friendly interface with customizable views.
    ```

  ### 1.3 Requirements List (MoSCoW Method)
    | ID | Requirement | Priority (Must/Should/Could) |
    |----|-------------|------------------------------|
    | REQ-001 | Infrastructure Usage Monitoring | Must Have |
    | REQ-002 | Automated Cost Optimization Recommendations | Must Have |
    | REQ-003 | Centralized Task Management for Infra Operations | Should Have |
    | REQ-004 | Real-time Alerting System | Must Have |


**Phase 1 Deliverables:**
- [ ] Software Requirements Specification (SRS)
- [ ] Requirements Traceability Matrix
- [ ] Risk Assessment

---

## Phase 2: System Design & Architecture (The "Architect")

  ### 2.1 Architecture Decision Records (ADR)
    **Architecture Pattern:** Microservices
  ![architecture-diagram](https://drive.usercontent.google.com/download?id=16EHml8buU7ql9rariw93LKF77E9qOEo_&export=view&authuser=0)
  
    **Reasoning:**
    Chosen Microservices for scalability, flexibility in deployment, and ease of maintenance. This allows independent scaling of components based on load and facilitates continuous integration and delivery.
    
  ### 2.2 Database Design

    **Database Type:** PostgreSQL (Primary), Redis (Caching), DynamoDB (Time-series), Vector DB (Logs/Metrics Search)

    **Schema / ER Diagram:**

    #### **PostgreSQL - Core Application Data**

    **1. users**
    ```sql
    - user_id (PK, UUID)
    - email (VARCHAR(255), UNIQUE, NOT NULL)
    - password_hash (VARCHAR(255), NOT NULL)
    - full_name (VARCHAR(100))
    - role (ENUM: 'admin', 'devops_engineer', 'manager', 'viewer')
    - team_id (FK → teams)
    - created_at (TIMESTAMP DEFAULT NOW())
    - updated_at (TIMESTAMP DEFAULT NOW())
    - last_login (TIMESTAMP)
    - is_active (BOOLEAN DEFAULT true)
    - INDEX idx_email, idx_team_id
    ```

    **2. teams**
    ```sql
    - team_id (PK, UUID)
    - team_name (VARCHAR(100), NOT NULL)
    - department (VARCHAR(100))
    - manager_id (FK → users)
    - created_at (TIMESTAMP)
    - INDEX idx_team_name
    ```

    **3. cloud_accounts**
    ```sql
    - account_id (PK, UUID)
    - provider (ENUM: 'aws', 'azure', 'gcp', 'digitalocean')
    - account_name (VARCHAR(100), NOT NULL)
    - account_identifier (VARCHAR(255), UNIQUE) -- AWS Account ID, Azure Subscription ID
    - credentials_encrypted (TEXT) -- Encrypted access keys/credentials
    - region (VARCHAR(50))
    - team_id (FK → teams)
    - is_active (BOOLEAN DEFAULT true)
    - created_at (TIMESTAMP)
    - updated_at (TIMESTAMP)
    - INDEX idx_provider, idx_team_id
    ```

    **4. infrastructure_resources**
    ```sql
    - resource_id (PK, UUID)
    - account_id (FK → cloud_accounts)
    - resource_type (ENUM: 'ec2', 'rds', 's3', 'lambda', 'vm', 'storage', 'function')
    - resource_identifier (VARCHAR(255)) -- Instance ID, ARN, etc.
    - resource_name (VARCHAR(255))
    - region (VARCHAR(50))
    - status (ENUM: 'running', 'stopped', 'terminated', 'pending')
    - tags (JSONB) -- Flexible tagging system
    - configuration (JSONB) -- Resource-specific config
    - cost_per_hour (DECIMAL(10,4))
    - created_at (TIMESTAMP)
    - updated_at (TIMESTAMP)
    - last_scanned_at (TIMESTAMP)

    - UNIQUE(account_id, resource_identifier)
    - INDEX idx_account_id, idx_resource_type, idx_status
    - INDEX idx_tags USING GIN(tags)
    ```

    **5. cost_optimization_recommendations**
    ```sql
    - recommendation_id (PK, UUID)
    - resource_id (FK → infrastructure_resources)
    - recommendation_type (ENUM: 'right_sizing', 'reserved_instance', 'spot_instance', 'terminate_idle', 'storage_optimization')
    - current_cost (DECIMAL(10,2))
    - potential_savings (DECIMAL(10,2))
    - confidence_score (DECIMAL(3,2)) -- 0.00 to 1.00
    - recommendation_details (JSONB)
    - status (ENUM: 'pending', 'approved', 'applied', 'rejected', 'expired')
    - created_at (TIMESTAMP)
    - expires_at (TIMESTAMP)
    - applied_at (TIMESTAMP)
    - applied_by (FK → users, nullable)
    - INDEX idx_resource_id, idx_status, idx_created_at
    ```

    **6. alerts**
    ```sql
    - alert_id (PK, UUID)
    - resource_id (FK → infrastructure_resources, nullable)
    - alert_type (ENUM: 'threshold_exceeded', 'cost_spike', 'resource_idle', 'security_issue', 'health_check_failed')
    - severity (ENUM: 'critical', 'high', 'medium', 'low', 'info')
    - title (VARCHAR(255))
    - description (TEXT)
    - threshold_config (JSONB) -- CPU > 80%, Cost > $500/day
    - triggered_at (TIMESTAMP DEFAULT NOW())
    - acknowledged_at (TIMESTAMP)
    - acknowledged_by (FK → users, nullable)
    - resolved_at (TIMESTAMP)
    - resolved_by (FK → users, nullable)
    - notification_sent (BOOLEAN DEFAULT false)
    - INDEX idx_resource_id, idx_severity, idx_triggered_at, idx_resolved_at
    ```

    **7. infrastructure_tasks**
    ```sql
    - task_id (PK, UUID)
    - task_name (VARCHAR(255), NOT NULL)
    - task_type (ENUM: 'backup', 'snapshot', 'scaling', 'deployment', 'maintenance', 'cost_optimization')
    - resource_id (FK → infrastructure_resources, nullable)
    - account_id (FK → cloud_accounts)
    - assigned_to (FK → users, nullable)
    - created_by (FK → users)
    - priority (ENUM: 'urgent', 'high', 'medium', 'low')
    - status (ENUM: 'pending', 'in_progress', 'completed', 'failed', 'cancelled')
    - scheduled_at (TIMESTAMP)
    - started_at (TIMESTAMP)
    - completed_at (TIMESTAMP)
    - task_config (JSONB) -- Task-specific parameters
    - execution_logs (TEXT)
    - created_at (TIMESTAMP)
    - updated_at (TIMESTAMP)
    - INDEX idx_assigned_to, idx_status, idx_scheduled_at
    ```

    **8. automation_rules**
    ```sql
    - rule_id (PK, UUID)
    - rule_name (VARCHAR(255), NOT NULL)
    - account_id (FK → cloud_accounts)
    - trigger_type (ENUM: 'schedule', 'threshold', 'event', 'cost_based')
    - trigger_config (JSONB) -- Cron expression, threshold values
    - action_type (ENUM: 'stop_instance', 'start_instance', 'resize', 'snapshot', 'alert', 'approve_recommendation')
    - action_config (JSONB)
    - is_active (BOOLEAN DEFAULT true)
    - last_executed_at (TIMESTAMP)
    - execution_count (INTEGER DEFAULT 0)
    - created_by (FK → users)
    - created_at (TIMESTAMP)
    - updated_at (TIMESTAMP)
    - INDEX idx_account_id, idx_is_active, idx_trigger_type
    ```

    **9. audit_logs**
    ```sql
    - log_id (PK, UUID)
    - user_id (FK → users, nullable)
    - action (VARCHAR(100)) -- 'create_resource', 'delete_resource', 'apply_recommendation'
    - entity_type (VARCHAR(50)) -- 'resource', 'alert', 'task'
    - entity_id (UUID)
    - changes (JSONB) -- Before/after values
    - ip_address (INET)
    - user_agent (TEXT)
    - created_at (TIMESTAMP DEFAULT NOW())
    - INDEX idx_user_id, idx_created_at, idx_entity_type, idx_entity_id
    ```

    ---

    #### **DynamoDB - Time-Series Metrics Data**

    **Table: resource_metrics**
    ```
    Partition Key: resource_id (String)
    Sort Key: timestamp (Number) -- Unix timestamp

    Attributes:
    - metric_type (String) -- 'cpu', 'memory', 'disk', 'network'
    - metric_value (Number)
    - unit (String) -- 'percent', 'bytes', 'count'
    - tags (Map)

    TTL: 90 days (auto-deletion of old metrics)
    GSI: metric_type-timestamp-index (for querying by metric type)
    ```

    **Table: cost_data**
    ```
    Partition Key: account_id (String)
    Sort Key: date (String) -- 'YYYY-MM-DD'

    Attributes:
    - total_cost (Number)
    - cost_by_service (Map) -- {ec2: 150.50, rds: 80.25, s3: 20.10}
    - cost_by_resource (Map)
    - currency (String)

    TTL: 2 years
    ```

    ---

    #### **Redis - Caching & Real-time Data**

    **Key Patterns:**
    ```
    - user:session:{user_id} → Session data (TTL: 24h)
    - resource:status:{resource_id} → Current status cache (TTL: 5min)
    - dashboard:metrics:{user_id} → Cached dashboard data (TTL: 1min)
    - alert:count:{severity} → Real-time alert counts (TTL: 30s)
    - rate_limit:{user_id}:{endpoint} → API rate limiting (TTL: 1h)
    ```

    ---

    #### **Vector DB (Pinecone/Weaviate) - Logs & Intelligent Search**

    **Collection: infrastructure_logs**
    ```
    - vector_id (String)
    - text (String) -- Log message
    - embedding (Vector[1536]) -- OpenAI embedding
    - metadata:
      - resource_id (String)
      - timestamp (DateTime)
      - log_level (String)
      - source (String)
      - tags (Array)
    ```

    **Use Cases:**
    - Semantic search across infrastructure logs
    - Anomaly detection in log patterns
    - Similar issue finding for troubleshooting

    ---

    #### **Relationships:**
    ```
    users 1:N infrastructure_tasks (created_by)
    users 1:N infrastructure_tasks (assigned_to)
    users 1:1 teams (belongs_to)
    teams 1:N cloud_accounts
    cloud_accounts 1:N infrastructure_resources
    infrastructure_resources 1:N cost_optimization_recommendations
    infrastructure_resources 1:N alerts
    infrastructure_resources 1:N resource_metrics (DynamoDB)
    cloud_accounts 1:N cost_data (DynamoDB)
    users 1:N audit_logs
    ```

    ---

    #### **Indexes Strategy:**

    **PostgreSQL:**
    - B-tree indexes on foreign keys and frequently queried columns
    - GIN indexes on JSONB columns (tags, configuration)
    - Partial indexes: `WHERE is_active = true`, `WHERE status = 'running'`
    - Composite indexes: `(account_id, created_at)`, `(resource_type, status)`

    **DynamoDB:**
    - LSI for querying metrics by different time ranges
    - GSI for cross-partition queries (e.g., all resources of a specific type)

    ---

    #### **Data Retention Policies:**

    - **audit_logs:** 1 year (archive to S3 after 90 days)
    - **resource_metrics:** 90 days in DynamoDB, 2 years in cold storage
    - **alerts:** Keep for 6 months
    - **cost_data:** 2 years
    - **recommendations:** Delete after 30 days if expired/rejected

    ---

    #### **Backup & Disaster Recovery:**

    - PostgreSQL: Daily full backups, 5-minute WAL archiving
    - DynamoDB: Point-in-time recovery enabled, Cross-region replication
    - Redis: RDB snapshots every 6 hours, AOF for write-ahead logging

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
- [x] Architecture Diagram
- [x] Database Schema
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