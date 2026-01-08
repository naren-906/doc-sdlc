# Software Development Lifecycle (SDLC) - Complete Guide

> A comprehensive guide for software engineers to understand and master the SDLC process.

---

## Table of Contents

1. [Overview](#overview)
2. [Phase 1: Requirements Gathering & Analysis](#phase-1-requirements-gathering--analysis)
3. [Phase 2: System Design & Architecture](#phase-2-system-design--architecture)
4. [Phase 3: Implementation (Coding)](#phase-3-implementation-coding)
5. [Phase 4: Testing](#phase-4-testing)
6. [Phase 5: Deployment](#phase-5-deployment)
7. [Phase 6: Maintenance & Operations](#phase-6-maintenance--operations)
8. [Phase 7: Iteration & Evolution](#phase-7-iteration--evolution)
9. [SDLC Models](#sdlc-models)
10. [Key Takeaways](#key-takeaways)
11. [Resources](#resources)

---

## Overview

The **Software Development Lifecycle (SDLC)** is a structured process that produces high-quality software in the shortest possible time. It provides a well-defined sequence of phases that guide teams from initial concept to deployment and maintenance.

### Why SDLC Matters

- **Predictability**: Clear phases help estimate timelines and costs
- **Quality**: Structured approach catches issues early
- **Communication**: Common framework for all stakeholders
- **Risk Management**: Early identification and mitigation of risks
- **Documentation**: Creates audit trail and knowledge base

### SDLC Visual Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    SOFTWARE DEVELOPMENT LIFECYCLE                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│    1. REQUIREMENTS ──→ 2. DESIGN ──→ 3. IMPLEMENT                │
│          │                                    │                  │
│          │         ┌──────────────────────────┘                  │
│          │         │                                             │
│          │         ▼                                             │
│    7. ITERATE ←── 6. MAINTAIN ←── 5. DEPLOY ←── 4. TEST         │
│          │                                                       │
│          └─────────────────→ (Repeat)                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Requirements Gathering & Analysis

### 📋 What Happens

This phase involves understanding **what** the software needs to do by collecting requirements from stakeholders, users, and business analysts.

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Active Participation** | Engage in stakeholder meetings, not just passive listening |
| **Ask "Why" Repeatedly** | Understand the root need, not just surface requests |
| **Document Assumptions** | Explicitly state and validate assumptions early |
| **Create User Stories** | Write stories with clear acceptance criteria |
| **Identify Edge Cases** | Think about scenarios stakeholders might not consider |

### Best Practices

| Practice | Description |
|----------|-------------|
| Use requirement templates | Standardized formats like IEEE 830 SRS |
| Prioritize with MoSCoW | Must, Should, Could, Won't have |
| Create prototypes early | Low-fidelity mockups to validate understanding |
| Maintain traceability | Link requirements to features and tests |
| Version control requirements | Track changes and who requested them |

### User Story Template

```
As a [type of user],
I want [an action or feature],
So that [benefit/value].

Acceptance Criteria:
- Given [context], when [action], then [outcome]
- Given [context], when [action], then [outcome]
```

### Example User Story

```
As a mobile banking user,
I want to transfer money using my fingerprint,
So that I can complete transactions quickly and securely.

Acceptance Criteria:
- Given I'm on the transfer screen, when I place my finger on the sensor, then the transfer is authenticated
- Given the fingerprint doesn't match, when I try to transfer, then I see an error and can retry up to 3 times
- Given I've exceeded retry attempts, when I try again, then I'm locked out for 30 minutes
```

### Real-World Example: Spotify's "Discover Weekly"

- **Requirement**: Users want personalized music recommendations
- **Analysis**: Engineers identified that users wanted *fresh* recommendations, not just similar songs they already liked
- **Key Insight**: Users wanted discovery, not just similarity
- **Outcome**: Weekly playlist that feels personal, updated every Monday
- **Success Metric**: 40 million users in first year

### Common Pitfalls & Issues

| Pitfall | Real Issue Faced | Solution |
|---------|------------------|----------|
| **Scope creep** | Netflix initially tried to add too many social features, delaying core improvements | Strict change control process with impact analysis |
| **Vague requirements** | "Make it fast" without defining metrics led to endless optimization | Define measurable criteria (e.g., <200ms response time) |
| **Missing stakeholders** | Instagram didn't initially consult advertisers, causing later friction | Comprehensive stakeholder mapping at project start |
| **Gold plating** | Engineers adding unrequested "cool" features | Stick to documented requirements only |
| **Analysis paralysis** | Over-analyzing requirements delays development | Time-box analysis phase, use prototypes to validate |
| **Assuming not asking** | Developers assumed user needs without validation | Conduct user interviews and surveys |

### Deliverables Checklist

- [ ] Software Requirements Specification (SRS)
- [ ] User stories with acceptance criteria
- [ ] Use case diagrams
- [ ] Requirements traceability matrix
- [ ] Stakeholder sign-off documentation
- [ ] Project scope document
- [ ] Risk assessment

---

## Phase 2: System Design & Architecture

### 🎨 What Happens

Transform requirements into a technical blueprint. Define the system's structure, components, interfaces, and data flow.

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Start with Constraints** | Budget, timeline, team skills, scalability needs |
| **Design for Failure** | Assume components will fail and plan accordingly |
| **Keep it Simple (KISS)** | Avoid over-engineering for hypothetical future needs |
| **Document Decisions** | Use Architecture Decision Records (ADRs) |
| **Think Observability** | Plan monitoring and debugging from the start |
| **Security by Design** | Build security in, don't bolt it on later |

### Best Practices

```
✅ Create multiple design options before choosing
✅ Use established patterns (microservices, event-driven, etc.)
✅ Design APIs contract-first
✅ Plan for horizontal scaling
✅ Security by design, not as an afterthought
✅ Conduct design reviews with peers
✅ Consider operational complexity
✅ Document trade-offs explicitly
```

### Architecture Decision Record (ADR) Template

```markdown
# ADR-001: [Title]

## Status
[Proposed | Accepted | Deprecated | Superseded]

## Context
What is the issue that we're seeing that is motivating this decision?

## Decision
What is the change that we're proposing and/or doing?

## Consequences
What becomes easier or more difficult to do because of this change?

## Alternatives Considered
What other options were evaluated?
```

### Common Architecture Patterns

#### 1. Monolithic Architecture
```
┌─────────────────────────────────────┐
│            APPLICATION              │
│  ┌─────┐  ┌─────┐  ┌─────┐         │
│  │ UI  │  │Logic│  │ Data│         │
│  └─────┘  └─────┘  └─────┘         │
└─────────────────────────────────────┘
         │
         ▼
    ┌─────────┐
    │Database │
    └─────────┘
```
**When to use**: Small teams, simple applications, rapid prototyping

#### 2. Microservices Architecture
```
┌─────────┐   ┌─────────┐   ┌─────────┐
│ Service │   │ Service │   │ Service │
│    A    │   │    B    │   │    C    │
└────┬────┘   └────┬────┘   └────┬────┘
     │             │             │
     ▼             ▼             ▼
┌─────────┐   ┌─────────┐   ┌─────────┐
│   DB A  │   │   DB B  │   │   DB C  │
└─────────┘   └─────────┘   └─────────┘
```
**When to use**: Large teams, complex domains, independent scaling needs

#### 3. Event-Driven Architecture
```
┌──────────┐     ┌──────────────┐     ┌──────────┐
│ Producer │────▶│ Event Queue  │────▶│ Consumer │
└──────────┘     └──────────────┘     └──────────┘
                        │
                        ▼
                 ┌──────────┐
                 │ Consumer │
                 └──────────┘
```
**When to use**: Asynchronous processing, decoupled systems, high throughput

### Real-World Example: Twitter's Architecture Evolution

| Phase | Architecture | Problem | Solution |
|-------|-------------|---------|----------|
| **2006-2009** | Monolithic Ruby on Rails | "Fail Whale" during high traffic | Couldn't scale |
| **2010-2012** | Service-oriented | Complex dependencies | Introduced microservices |
| **2012-Present** | Microservices | Performance issues | Separate services for timeline, tweets, users |

**Current Scale**: Handles 500 million tweets/day

### Common Pitfalls & Issues

| Pitfall | Real Issue Faced | Solution |
|---------|------------------|----------|
| **Over-engineering** | Premature optimization at Friendster led to delays and eventual failure | Build for current scale + 10x, not 1000x |
| **Ignoring NFRs** | Healthcare.gov crashed on launch (2013) due to untested load | Load test with realistic traffic patterns |
| **Tight coupling** | eBay's early monolith was extremely hard to modify | Design loosely coupled services with clear interfaces |
| **No disaster recovery** | GitLab lost 6 hours of production data (2017) | Design backup and recovery from day 1 |
| **Distributed monolith** | Microservices without proper boundaries | Ensure services are truly independent |
| **Ignoring team structure** | Architecture didn't match team organization | Apply Conway's Law consciously |

### Deliverables Checklist

- [ ] High-level architecture diagram
- [ ] Component/service specifications
- [ ] Database schema design
- [ ] API contracts/specifications
- [ ] Security architecture
- [ ] Infrastructure requirements
- [ ] Architecture Decision Records (ADRs)
- [ ] Non-functional requirements mapping

---

## Phase 3: Implementation (Coding)

    ### 💻 What Happens

    Developers write the actual code based on design specifications. This is where the rubber meets the road.

    ### How Experienced Engineers Approach It

    | Approach | Description |
    |----------|-------------|
    | **Code for Humans First** | Readability over cleverness |
    | **Small, Incremental Commits** | Meaningful commit messages |
    | **Test as You Go** | Don't wait until the end |
    | **Refactor Continuously** | Leave code cleaner than you found it |
    | **Pair Programming** | For complex features and knowledge sharing |
    | **Code Reviews** | Learning opportunities, not gatekeeping |

    ### Best Practices

    #### Code Quality Example

    ```python
    # ❌ Bad: Magic numbers, unclear intent, no type hints
    def calc(x):
        return x * 0.0825 + x

    # ✅ Good: Self-documenting, testable, typed
    from decimal import Decimal
    from typing import Final

    TAX_RATE: Final[Decimal] = Decimal("0.0825")

    def calculate_total_with_tax(subtotal: Decimal) -> Decimal:
        """
        Calculate total price including sales tax.
        
        Args:
            subtotal: The pre-tax amount
            
        Returns:
            The total amount including tax
            
        Example:
            >>> calculate_total_with_tax(Decimal("100.00"))
            Decimal('108.25')
        """
        tax_amount = subtotal * TAX_RATE
        return subtotal + tax_amount
    ```

    #### Git Commit Message Convention

    ```
    <type>(<scope>): <subject>

    <body>

    <footer>

    Types:
    - feat: New feature
    - fix: Bug fix
    - docs: Documentation
    - style: Formatting
    - refactor: Code restructuring
    - test: Adding tests
    - chore: Maintenance

    Example:
    feat(auth): add fingerprint authentication

    Implement biometric authentication for mobile banking transfers.
    Includes retry logic and lockout mechanism.

    Closes #123
    ```

    ### Code Review Checklist

    ```markdown
    ## Functionality
    - [ ] Code does what it's supposed to do
    - [ ] Edge cases are handled
    - [ ] Error handling is appropriate

    ## Code Quality
    - [ ] Code is readable and self-documenting
    - [ ] No code duplication (DRY)
    - [ ] Functions are small and focused
    - [ ] Naming is clear and consistent

    ## Testing
    - [ ] Unit tests are included
    - [ ] Tests cover happy path and edge cases
    - [ ] Tests are readable and maintainable

    ## Security
    - [ ] No hardcoded secrets
    - [ ] Input validation is present
    - [ ] No SQL injection vulnerabilities
    - [ ] Authentication/authorization checked

    ## Performance
    - [ ] No obvious performance issues
    - [ ] Database queries are optimized
    - [ ] No N+1 query problems
    ```

    ### Real-World Example: Google's Code Review Culture

    | Metric | Value |
    |--------|-------|
    | Codebase size | 2 billion lines of code |
    | Review requirement | Every change needs at least one reviewer |
    | Average review time | < 24 hours |
    | Special reviews | Code readability reviews for new employees |

    **Result**: Consistent, high-quality codebase across thousands of engineers

    ### Common Pitfalls & Issues

    | Pitfall | Real Issue Faced | Solution |
    |---------|------------------|----------|
    | **Technical debt** | Facebook's PHP codebase became unmaintainable | Regular refactoring sprints, 20% rule |
    | **Not handling errors** | Knight Capital lost $440M in 45 min (2012) due to unhandled edge case | Defensive programming, fail-safe defaults |
    | **Hardcoded configs** | Environment-specific code broke deployments | Use environment variables and config files |
    | **Ignoring performance** | Slack's initial slow startup times frustrated users | Profile and optimize hot paths early |
    | **Copy-paste coding** | Duplicated bugs across codebase | Extract common functionality, DRY principle |
    | **No code reviews** | Bugs shipped to production undetected | Mandatory peer reviews for all changes |

    ### Deliverables Checklist

    - [ ] Source code in version control
    - [ ] Unit tests with adequate coverage
    - [ ] Code documentation (inline and API docs)
    - [ ] Build scripts and configurations
    - [ ] Development environment setup guide
    - [ ] Code review completion

    ---

## Phase 4: Testing

### 🧪 What Happens

Verify that the software works correctly, handles edge cases, and meets requirements.

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Test Pyramid** | Many unit tests, fewer integration tests, even fewer E2E tests |
| **Test Behavior** | Test behavior, not implementation details |
| **Test Before Fixing** | Write test before fixing bug (prevents regression) |
| **Automate Everything** | Manual testing doesn't scale |
| **Production-like Environments** | Test in environments that mirror production |

### The Test Pyramid

```
┌─────────────────────────────────────────────────┐
│                 E2E Tests (10%)                 │  ← Slow, expensive, realistic
│          (Full user journeys)                   │
├─────────────────────────────────────────────────┤
│           Integration Tests (20%)               │  ← Test component interactions
│        (API, database, services)                │
├─────────────────────────────────────────────────┤
│              Unit Tests (70%)                   │  ← Fast, focused, many
│         (Individual functions)                  │
└─────────────────────────────────────────────────┘
```

### Types of Testing

| Type | Purpose | Example |
|------|---------|---------|
| **Unit Testing** | Test individual functions/methods | Test that `calculateTax()` returns correct value |
| **Integration Testing** | Test component interactions | Test API endpoint with database |
| **End-to-End (E2E)** | Test complete user flows | Test entire checkout process |
| **Performance Testing** | Test speed and scalability | Load test with 10,000 concurrent users |
| **Security Testing** | Find vulnerabilities | Penetration testing, OWASP checks |
| **Usability Testing** | Test user experience | User testing sessions |
| **Regression Testing** | Ensure old features still work | Re-run all tests after changes |
| **Smoke Testing** | Quick sanity check | Basic functionality after deployment |

### Test Example (Python with pytest)

```python
import pytest
from decimal import Decimal
from myapp.pricing import calculate_total_with_tax, TAX_RATE

class TestCalculateTotalWithTax:
    """Tests for the calculate_total_with_tax function."""
    
    def test_basic_calculation(self):
        """Should correctly calculate tax for a simple amount."""
        result = calculate_total_with_tax(Decimal("100.00"))
        assert result == Decimal("108.25")
    
    def test_zero_amount(self):
        """Should handle zero subtotal."""
        result = calculate_total_with_tax(Decimal("0"))
        assert result == Decimal("0")
    
    def test_large_amount(self):
        """Should handle large amounts without overflow."""
        result = calculate_total_with_tax(Decimal("1000000.00"))
        expected = Decimal("1000000.00") * (1 + TAX_RATE)
        assert result == expected
    
    def test_decimal_precision(self):
        """Should maintain decimal precision."""
        result = calculate_total_with_tax(Decimal("99.99"))
        # Verify precision is maintained
        assert result == Decimal("108.2391675")
    
    @pytest.mark.parametrize("subtotal,expected", [
        (Decimal("50.00"), Decimal("54.125")),
        (Decimal("200.00"), Decimal("216.50")),
        (Decimal("0.01"), Decimal("0.01082500")),
    ])
    def test_various_amounts(self, subtotal, expected):
        """Should calculate correctly for various amounts."""
        result = calculate_total_with_tax(subtotal)
        assert result == expected
```

### Real-World Example: Amazon's Testing Strategy

| Metric | Value |
|--------|-------|
| Deployment frequency | Every 11.7 seconds on average |
| Automated test coverage | 99% of bugs caught before production |
| Canary deployments | New code goes to 1% of servers first |
| A/B testing | Every feature tested with real users |

### Common Pitfalls & Issues

| Pitfall | Real Issue Faced | Solution |
|---------|------------------|----------|
| **No tests** | Therac-25 radiation machine killed patients (1980s) due to untested race condition | Mandatory test coverage requirements |
| **Flaky tests** | Tests pass/fail randomly, team ignores them | Fix or delete flaky tests immediately |
| **Testing only happy path** | Ariane 5 rocket exploded (1996) due to untested integer overflow | Test edge cases and error conditions |
| **Testing in isolation** | Works on dev machine, fails in production | Use containers, test in staging environment |
| **Too many E2E tests** | Test suite takes hours to run, developers skip it | Follow test pyramid, more unit tests |
| **Testing implementation** | Tests break on refactoring even when behavior unchanged | Test behavior and outputs, not internals |

### Test Coverage Guidelines

| Type of Code | Recommended Coverage |
|--------------|---------------------|
| Critical business logic | 90-100% |
| API endpoints | 80-90% |
| Utility functions | 70-80% |
| UI components | 60-70% |
| Generated code | Can be lower |

### Deliverables Checklist

- [ ] Unit tests with coverage report
- [ ] Integration test suite
- [ ] E2E test scenarios
- [ ] Performance test results
- [ ] Security audit report
- [ ] Test plan documentation
- [ ] Bug tracking and resolution

---

## Phase 5: Deployment

### 🚀 What Happens

Release the software to production environments where real users can access it.

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Automate Deployments** | If it's manual, it's error-prone |
| **Deploy Frequently** | Smaller changes = smaller risks |
| **Always Have Rollback** | Things will go wrong |
| **Monitor During Deployment** | Watch metrics closely |
| **Start Low-Traffic** | Deploy during off-peak hours initially |

### Deployment Strategies

#### 1. Blue-Green Deployment
```
                    ┌─────────────────┐
                    │   Load Balancer │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              │              ▼
       ┌──────────┐          │       ┌──────────┐
       │   Blue   │ ◄────────┘       │  Green   │
       │ (Live)   │    Switch        │  (New)   │
       └──────────┘                  └──────────┘

Process:
1. Deploy new version to Green
2. Test Green environment
3. Switch traffic from Blue to Green
4. Keep Blue as rollback option
```

#### 2. Canary Deployment
```
┌─────────────────┐
│  Load Balancer  │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌───────┐ ┌───────┐
│  95%  │ │  5%   │
│  Old  │ │  New  │
│Version│ │Version│
└───────┘ └───────┘
    │         │
    └────┬────┘
         ▼
   Monitor metrics
   Gradually increase
   new version traffic
```

#### 3. Rolling Deployment
```
Time →
───────────────────────────────────────────►

Server 1: [Old] → [Updating] → [New] → [New] → [New]
Server 2: [Old] → [Old] → [Updating] → [New] → [New]
Server 3: [Old] → [Old] → [Old] → [Updating] → [New]
Server 4: [Old] → [Old] → [Old] → [Old] → [Updating]
```

#### 4. Feature Flags
```python
# Feature flag example
from featureflags import is_enabled

def get_recommendations(user):
    if is_enabled("new_recommendation_algorithm", user):
        return new_algorithm(user)
    else:
        return old_algorithm(user)
```

### CI/CD Pipeline Example

```yaml
# .github/workflows/deploy.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: |
          pip install -r requirements.txt
          pytest --cov=app tests/

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
      - name: Push to registry
        run: docker push myapp:${{ github.sha }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
      - name: Run smoke tests
        run: ./scripts/smoke-test.sh staging

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy canary (5%)
        run: ./scripts/canary-deploy.sh 5
      - name: Monitor for 10 minutes
        run: ./scripts/monitor-canary.sh
      - name: Full rollout
        run: ./scripts/canary-deploy.sh 100
```

### Real-World Example: Facebook's Deployment

| Feature | Description |
|---------|-------------|
| Methodology | Continuous Deployment with multiple daily releases |
| Gatekeeper | Feature flag system for gradual rollouts |
| Dark launches | Code deployed but not activated |
| Testing sequence | Employees → 1% users → 10% users → 100% users |
| Rollback time | < 10 minutes to any previous version |

### Common Pitfalls & Issues

| Pitfall | Real Issue Faced | Solution |
|---------|------------------|----------|
| **No rollback plan** | Cloudflare's 2019 outage took 27 min to fix | Automated rollback triggers on error rates |
| **Big bang releases** | Healthcare.gov's disastrous 2013 launch | Incremental rollouts with canary deployments |
| **Manual deployments** | Configuration errors at Knight Capital | Infrastructure as Code (Terraform, Ansible) |
| **Deploying on Friday** | Issues discovered over weekend with no staff | Deploy early in week, freeze Fridays |
| **No deployment checklist** | Missed steps caused outages | Automated deployment pipeline |
| **No smoke tests** | Broken deployments not caught immediately | Automated post-deployment verification |

### Deployment Checklist

```markdown
## Pre-Deployment
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Database migrations tested
- [ ] Feature flags configured
- [ ] Rollback plan documented
- [ ] On-call team notified

## During Deployment
- [ ] Monitor error rates
- [ ] Watch response times
- [ ] Check resource utilization
- [ ] Verify key user flows

## Post-Deployment
- [ ] Smoke tests passing
- [ ] No increase in error rates
- [ ] Performance metrics normal
- [ ] User-facing features verified
- [ ] Documentation updated
```

### Deliverables Checklist

- [ ] Deployment pipeline (CI/CD)
- [ ] Infrastructure as Code
- [ ] Deployment runbook
- [ ] Rollback procedures
- [ ] Environment configurations
- [ ] Monitoring dashboards

---

## Phase 6: Maintenance & Operations

### 🔧 What Happens

Monitor, update, fix bugs, and improve the software after it's in production. This is the longest phase (often **60-80% of total effort**).

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Observability First** | Logs, metrics, traces - you can't fix what you can't see |
| **On-call Rotations** | Fair distribution of after-hours support |
| **Runbooks** | Document how to handle common incidents |
| **Blameless Post-mortems** | Learn from failures without finger-pointing |
| **Continuous Improvement** | Regular performance tuning |

### The Three Pillars of Observability

```
┌─────────────────────────────────────────────────────────────┐
│                     OBSERVABILITY                            │
├───────────────────┬───────────────────┬─────────────────────┤
│       LOGS        │      METRICS      │       TRACES        │
├───────────────────┼───────────────────┼─────────────────────┤
│ What happened     │ Aggregated data   │ Request flow        │
│ Detailed events   │ over time         │ across services     │
│ Debugging         │ Alerting          │ Performance         │
│                   │ Dashboards        │ analysis            │
├───────────────────┼───────────────────┼─────────────────────┤
│ ELK Stack         │ Prometheus        │ Jaeger              │
│ Splunk            │ Datadog           │ Zipkin              │
│ CloudWatch        │ Grafana           │ AWS X-Ray           │
└───────────────────┴───────────────────┴─────────────────────┘
```

### The Four Golden Signals (Google SRE)

```
📊 Monitor These Four Signals:

┌─────────────────────────────────────────────────────────────┐
│                                                              │
│  1. LATENCY                                                  │
│     └── How long requests take                               │
│         • P50, P95, P99 response times                      │
│         • Differentiate successful vs failed requests       │
│                                                              │
│  2. TRAFFIC                                                  │
│     └── How much demand on the system                       │
│         • Requests per second                                │
│         • Concurrent users                                   │
│                                                              │
│  3. ERRORS                                                   │
│     └── Rate of failed requests                             │
│         • HTTP 5xx errors                                    │
│         • Failed database queries                            │
│                                                              │
│  4. SATURATION                                              │
│     └── How "full" the system is                            │
│         • CPU utilization                                    │
│         • Memory usage                                       │
│         • Disk I/O                                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Incident Response Process

```
┌──────────────────────────────────────────────────────────────┐
│                   INCIDENT RESPONSE FLOW                      │
└──────────────────────────────────────────────────────────────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   DETECT     │ ← Monitoring alerts
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   TRIAGE     │ ← Severity classification
                    └──────┬───────┘   (SEV1, SEV2, SEV3)
                           │
                           ▼
                    ┌──────────────┐
                    │   RESPOND    │ ← Incident commander assigned
                    └──────┬───────┘   Communication started
                           │
                           ▼
                    ┌──────────────┐
                    │   MITIGATE   │ ← Stop the bleeding
                    └──────┬───────┘   (rollback, scale, redirect)
                           │
                           ▼
                    ┌──────────────┐
                    │    FIX       │ ← Root cause fix
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   REVIEW     │ ← Post-mortem
                    └──────────────┘   (blameless)
```

### Post-Mortem Template

```markdown
# Incident Post-Mortem: [Title]

## Summary
Brief description of what happened.

## Impact
- Duration: X hours
- Users affected: Y
- Revenue impact: $Z

## Timeline (all times UTC)
- 14:00 - Alert triggered for high error rate
- 14:05 - On-call engineer acknowledged
- 14:15 - Root cause identified
- 14:30 - Fix deployed
- 14:35 - Systems recovered

## Root Cause
Technical explanation of what caused the incident.

## What Went Well
- Quick detection
- Clear communication

## What Went Wrong
- Slow rollback
- Missing runbook for this scenario

## Action Items
| Action | Owner | Due Date |
|--------|-------|----------|
| Add monitoring for X | @engineer | 2025-01-15 |
| Update runbook | @oncall | 2025-01-10 |
| Add circuit breaker | @team | 2025-01-20 |

## Lessons Learned
What we learned and how we'll prevent this in the future.
```

### Real-World Example: Netflix's Chaos Engineering

| Tool | Purpose |
|------|---------|
| **Chaos Monkey** | Randomly kills production servers |
| **Chaos Kong** | Simulates entire region failures |
| **Latency Monkey** | Introduces artificial delays |
| **Conformity Monkey** | Finds instances not adhering to best practices |

**Result**: 99.99% uptime even during AWS outages

### Common Pitfalls & Issues

| Pitfall | Real Issue Faced | Solution |
|---------|------------------|----------|
| **Alert fatigue** | Too many alerts, team ignores them all | Tune alerts, prioritize actionable ones only |
| **No documentation** | Only original developer knows how it works | Documentation as part of Definition of Done |
| **Ignoring tech debt** | MySpace couldn't adapt to changes, lost to Facebook | Allocate 20% time for maintenance |
| **No monitoring** | Discovered issues from user complaints (too late) | Proactive monitoring with dashboards |
| **Blame culture** | Engineers hide mistakes, problems repeat | Blameless post-mortems focus on systems |
| **Hero culture** | One person always fixes everything | On-call rotations, knowledge sharing |

### Deliverables Checklist

- [ ] Monitoring and alerting setup
- [ ] Runbooks for common incidents
- [ ] On-call schedule and escalation paths
- [ ] Performance baselines
- [ ] Backup and recovery procedures
- [ ] Security patch management process
- [ ] SLA/SLO definitions

---

## Phase 7: Iteration & Evolution

### 📈 What Happens

Gather feedback, analyze metrics, and plan the next version. Software is never "done."

### How Experienced Engineers Approach It

| Approach | Description |
|----------|-------------|
| **Data-Driven Decisions** | Use metrics, not opinions |
| **User Feedback Loops** | In-app feedback, surveys, analytics |
| **Competitive Analysis** | What are others doing better? |
| **Technical Health Checks** | Address accumulated tech debt |
| **Retrospectives** | What worked? What didn't? |

### Feedback Collection Methods

```
┌─────────────────────────────────────────────────────────────┐
│                    FEEDBACK SOURCES                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  QUANTITATIVE                    QUALITATIVE                │
│  ────────────                    ───────────                │
│  • Analytics data                • User interviews          │
│  • A/B test results             • Support tickets           │
│  • Error rates                   • App store reviews        │
│  • Performance metrics           • Social media mentions    │
│  • Feature usage stats           • User surveys             │
│  • Funnel analysis              • Usability testing        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Sprint Retrospective Template

```markdown
# Sprint Retrospective - Sprint [X]

## What Went Well 🎉
- [Team achievements]
- [Successful practices]
- [Positive outcomes]

## What Could Be Improved 🔧
- [Pain points]
- [Blockers]
- [Inefficiencies]

## Action Items for Next Sprint 📋
| Action | Owner | Deadline |
|--------|-------|----------|
| [Action 1] | [Name] | [Date] |
| [Action 2] | [Name] | [Date] |

## Kudos 👏
- Shoutout to [person] for [achievement]
```

### Real-World Example: Slack's Continuous Evolution

| Stage | Description |
|-------|-------------|
| **Origin** | Started as internal tool for a gaming company (Tiny Speck) |
| **Pivot** | Realized the chat tool was more valuable than the game |
| **Evolution** | Continuous improvement based on user feedback |
| **Feature additions** | Threads, reactions, apps - all based on user requests |
| **Acquisition** | Bought by Salesforce for $27.7 billion |

### Technical Debt Management

```
┌─────────────────────────────────────────────────────────────┐
│                  TECH DEBT QUADRANT                          │
├──────────────────────────┬──────────────────────────────────┤
│                          │                                   │
│   RECKLESS               │   PRUDENT                        │
│   "We don't have time    │   "We must ship now and deal     │
│    for design"           │    with consequences"            │
│                          │                                   │
│   DELIBERATE ────────────┼─────────────────────────────────│
│                          │                                   │
│   "What's layering?"     │   "Now we know how we should    │
│                          │    have done it"                  │
│   INADVERTENT            │   INADVERTENT                    │
│                          │                                   │
└──────────────────────────┴──────────────────────────────────┘

Priority for paying down tech debt:
1. High impact on velocity + Easy to fix → Do now
2. High impact on velocity + Hard to fix → Plan for it
3. Low impact + Easy to fix → Do when nearby
4. Low impact + Hard to fix → Maybe never
```

### Deliverables Checklist

- [ ] User feedback analysis
- [ ] Feature usage analytics
- [ ] Technical debt inventory
- [ ] Sprint retrospective notes
- [ ] Roadmap updates
- [ ] Competitive analysis
- [ ] Next iteration planning

---

## SDLC Models

### Comparison of Popular Models

| Model | Best For | Pros | Cons |
|-------|----------|------|------|
| **Waterfall** | Fixed requirements, regulated industries | Clear milestones, good documentation | Inflexible, late testing |
| **Agile/Scrum** | Evolving requirements, fast-paced | Flexible, early feedback | Scope creep, needs discipline |
| **Kanban** | Continuous flow, maintenance | Visual, limits WIP | Less structured |
| **DevOps** | Continuous delivery | Fast deployments, automation | Complex setup |
| **Spiral** | High-risk projects | Risk-focused, iterative | Complex, expensive |

### Waterfall Model

```
┌─────────────┐
│ Requirements│
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Design    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│Implementation│
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Testing   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Deployment  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Maintenance │
└─────────────┘
```

### Agile/Scrum Model

```
     ┌───────────────────────────────────────────┐
     │              PRODUCT BACKLOG               │
     └───────────────────┬───────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│                         SPRINT (2-4 weeks)                    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐          │
│  │  Plan   │→ │ Develop │→ │  Test   │→ │ Review  │          │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘          │
│                                              │                │
│                    Daily Standups            ▼                │
│                                         ┌─────────┐           │
│                                         │  Retro  │           │
│                                         └─────────┘           │
└──────────────────────────────────────────────────────────────┘
                         │
                         ▼
                 ┌───────────────┐
                 │   SHIPPABLE   │
                 │   INCREMENT   │
                 └───────────────┘
                         │
                         └──────→ (Next Sprint)
```

---

## Key Takeaways

### For New Engineers

```
┌─────────────────────────────────────────────────────────────┐
│                    TOP 10 TAKEAWAYS                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Communication is as important as coding                 │
│     └── Most failures are people problems, not technical    │
│                                                              │
│  2. Embrace automation                                       │
│     └── Anything you do twice should be automated           │
│                                                              │
│  3. Fail fast, learn faster                                 │
│     └── Small mistakes early prevent big disasters later    │
│                                                              │
│  4. Documentation is a gift to your future self            │
│     └── Write it as you go, not after                       │
│                                                              │
│  5. Perfect is the enemy of good                            │
│     └── Ship, get feedback, iterate                         │
│                                                              │
│  6. Security and testing are not optional                   │
│     └── Build them in from the start                        │
│                                                              │
│  7. Learn from production                                    │
│     └── That's where the real lessons are                   │
│                                                              │
│  8. Code for the next person                                │
│     └── That person might be you in 6 months                │
│                                                              │
│  9. Understand the business                                  │
│     └── Technical decisions should serve business goals     │
│                                                              │
│  10. Never stop learning                                     │
│      └── Technology evolves, so should you                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Skills to Develop at Each Phase

| Phase | Technical Skills | Soft Skills |
|-------|-----------------|-------------|
| Requirements | Domain modeling, UML | Active listening, questioning |
| Design | System design, patterns | Trade-off analysis, communication |
| Implementation | Programming, testing | Time management, collaboration |
| Testing | Test frameworks, automation | Attention to detail, patience |
| Deployment | CI/CD, cloud platforms | Risk management, planning |
| Maintenance | Debugging, monitoring | Problem-solving, prioritization |
| Iteration | Data analysis, metrics | Feedback incorporation, adaptability |

---

## Resources

### Books
- "Clean Code" by Robert C. Martin
- "The Pragmatic Programmer" by David Thomas & Andrew Hunt
- "Designing Data-Intensive Applications" by Martin Kleppmann
- "Site Reliability Engineering" by Google
- "The Phoenix Project" by Gene Kim

### Online Resources
- [Martin Fowler's Blog](https://martinfowler.com/)
- [Google SRE Book](https://sre.google/sre-book/table-of-contents/)
- [The Twelve-Factor App](https://12factor.net/)
- [OWASP Security Guidelines](https://owasp.org/)

### Tools by Phase

| Phase | Tools |
|-------|-------|
| Requirements | Jira, Confluence, Miro, Figma |
| Design | Draw.io, Lucidchart, PlantUML |
| Implementation | Git, VS Code, IDEs |
| Testing | Jest, pytest, Selenium, JMeter |
| Deployment | GitHub Actions, Jenkins, ArgoCD |
| Maintenance | Datadog, Grafana, PagerDuty |

---

*Last Updated: January 2026*
*Author: Software Engineering Knowledge Base*
