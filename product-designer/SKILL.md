---
name: product-designer
description: Automated product planning and design. Transform requirements into comprehensive product plans including architecture, features, UX design, technical decisions, and implementation roadmaps. Use this skill when you need to plan and design products from requirements.
license: MIT
---

This skill transforms user requirements into complete product planning and design documents. It provides systematic analysis, architectural decisions, feature specifications, and actionable implementation roadmaps.

The user invokes this skill by providing requirements, goals, or problems to solve. They may describe a new product, feature, system, or improvement opportunity.

## Product Planning Framework

### Phase 1: Requirements Analysis & Clarification

Before designing, deeply understand the problem space:

**Understand the Context:**
- What problem are we solving?
- Who are the target users?
- What are the user's goals and pain points?
- What is the current solution (if any)?
- What are the constraints (time, budget, technical)?
- What defines success?

**Ask Clarifying Questions:**
If requirements are unclear, ask about:
- Target audience and user personas
- Core use cases and scenarios
- Success metrics and KPIs
- Technical constraints and preferences
- Integration requirements
- Scale and performance expectations
- Security and compliance needs
- Budget and timeline constraints

**Categorize Requirements:**

| Type | Description | Examples |
|------|-------------|----------|
| **Functional** | What the system must do | User authentication, data processing, reporting |
| **Non-Functional** | Quality attributes | Performance, scalability, security, usability |
| **Business** | Business goals and constraints | Revenue targets, cost limits, time-to-market |
| **Technical** | Technical requirements | Tech stack preferences, APIs, databases |
| **Compliance** | Regulatory requirements | GDPR, SOC2, accessibility standards |

### Phase 2: Product Vision & Strategy

Define the product's direction and positioning:

**Product Vision Statement:**
```
Create a [product type] that helps [target users] [solve core problem]
by [unique approach/innovation].
```

**Product Positioning:**
- Market category and competitors
- Differentiation and unique value proposition
- Target market segment
- Pricing strategy (if applicable)

**Success Metrics:**
- User engagement metrics (DAU, MAU, retention)
- Business metrics (revenue, conversion, churn)
- Technical metrics (latency, uptime, performance)
- Quality metrics (NPS, satisfaction, bug rate)

### Phase 3: User Experience Design

**User Personas:**
Define 2-4 primary user personas:

```
Persona Name: [Descriptive Name]
Role: [Job title or role]
Goals: [What they want to achieve]
Pain Points: [Current frustrations]
Skills: [Technical proficiency level]
Behaviors: [Usage patterns and preferences]
```

**User Journeys:**
Map key user flows:

```
Use Case: [User goal]
Trigger: [What starts the journey]
Steps:
  1. [Action] → [System response]
  2. [Action] → [System response]
  ...
Outcome: [End result]
Emotional State: [User feelings at each step]
```

**Information Architecture:**
- Site map or navigation structure
- Content hierarchy
- Key screens/pages
- Data flow between components

**UX Principles:**
- Core design principles (e.g., simplicity, speed, clarity)
- Interaction patterns (e.g., progressive disclosure, immediate feedback)
- Visual style direction (minimal, playful, professional, etc.)
- Accessibility considerations (WCAG compliance, screen readers)

### Phase 4: Functional Architecture

**System Components:**

```
┌─────────────────────────────────────────┐
│           User Interface Layer          │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ Web App │ │  Mobile │ │   API   │   │
│  └─────────┘ └─────────┘ └─────────┘   │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│          Application Layer              │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ Auth    │ │  Core   │ │ Business│   │
│  │ Service │ │  Logic  │ │  Rules  │   │
│  └─────────┘ └─────────┘ └─────────┘   │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│            Data Layer                   │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ Primary │ │  Cache  │ │ File    │   │
│  │  DB     │ │  Layer  │ │ Storage │   │
│  └─────────┘ └─────────┘ └─────────┘   │
└─────────────────────────────────────────┘
```

**Module Breakdown:**

| Module | Responsibilities | Key Features |
|--------|------------------|--------------|
| Authentication | User identity, access control | Login, register, password reset, 2FA |
| [Feature Module] | [Business function] | [Specific capabilities] |

**Data Model:**
Define core entities and relationships:

```
User
├── id: UUID
├── email: string (unique)
├── profile: UserProfile
└── preferences: UserPreferences

UserProfile
├── userId: UUID (FK → User)
├── name: string
└── avatar: URL
```

**API Design:**
Key endpoints:

```
POST   /api/auth/register        - Create new account
POST   /api/auth/login           - Authenticate
GET    /api/users/:id            - Get user details
PUT    /api/users/:id            - Update user
DELETE /api/users/:id            - Delete account
```

### Phase 5: Technical Decisions

**Technology Stack Selection:**

Recommend based on:
- Team expertise and preferences
- Project requirements (scale, performance, features)
- Development speed vs. optimization
- Ecosystem and library support
- Long-term maintainability

**Frontend Options:**

| Option | Best For | Trade-offs |
|--------|----------|------------|
| **React + Next.js** | SEO-critical, full-stack apps | Server-side complexity |
| **React + SPA** | Rich interactive apps | SEO challenges |
| **Vue + Nuxt.js** | Progressive enhancement | Smaller ecosystem |
| **SvelteKit** | Performance-critical | Newer, fewer resources |
| **Server-rendered** | Simple content sites | Limited interactivity |

**Backend Options:**

| Option | Best For | Trade-offs |
|--------|----------|------------|
| **Node.js + Express/Fastify** | JS full-stack, real-time | Callback complexity |
| **Go + Gin/Echo** | High performance, microservices | Verbosity |
| **Python + FastAPI** | AI/ML integration, data processing | GIL limitations |
| **Rust + Actix/Axum** | Maximum performance, safety | Steep learning curve |
| **Java/Spring** | Enterprise systems | Boilerplate, resource heavy |

**Database Selection:**

| Type | Options | Best For |
|------|---------|----------|
| **Relational** | PostgreSQL, MySQL | Structured data, transactions |
| **Document** | MongoDB, CouchDB | Flexible schema, nested data |
| **Key-Value** | Redis, DynamoDB | Caching, simple queries |
| **Search** | Elasticsearch, Typesense | Full-text search |
| **Time-series** | InfluxDB, TimescaleDB | Metrics, logs |

**Architecture Patterns:**

```
Monolithic          - Simple, fast to develop
                     → Harder to scale at size

Microservices       - Independent scaling, polyglot
                     → Network complexity, distributed challenges

Serverless          - Auto-scaling, pay-per-use
                     → Cold starts, vendor lock-in

Event-Driven        - Loose coupling, async
                     → Event sourcing complexity, eventual consistency
```

**Infrastructure:**

```
Frontend:
  - Hosting: Vercel, Netlify, Cloudflare Pages
  - CDN: Cloudflare, AWS CloudFront
  - Analytics: Google Analytics, Plausible

Backend:
  - Hosting: AWS, GCP, Azure, Railway, Fly.io
  - Container: Docker, Kubernetes
  - CI/CD: GitHub Actions, GitLab CI

Data:
  - Primary DB: Managed service (RDS, Cloud SQL)
  - Cache: Redis (ElastiCache, Upstash)
  - Storage: S3, Cloud Storage, Backblaze

Monitoring:
  - Metrics: Prometheus, Datadog, New Relic
  - Logging: ELK, Loki, Cloud Logging
  - Tracing: OpenTelemetry, Jaeger
```

### Phase 6: Feature Prioritization

**MoSCoW Prioritization:**

| Priority | Features | Rationale |
|----------|----------|-----------|
| **Must Have** | Core functionality, MVP features | Essential for launch |
| **Should Have** | Important but not critical | High value, can defer |
| **Could Have** | Nice to have | Desirable if time permits |
| **Won't Have** | Out of scope | Explicitly deferred |

**Value vs. Effort Matrix:**

```
High Effort       High Value + High Effort      High Value + Low Effort
  ↑            (Strategic bets - plan carefully)  (Quick wins - do first)
  │
  │            Low Value + High Effort           Low Value + Low Effort
  │            (Avoid - re-evaluate need)        (Fill-ins - do if time)
  │
  └──────────────────────────────────────────────→ Low Effort

Examples:
  Quick wins: Authentication, basic CRUD
  Strategic bets: AI features, advanced analytics
  Fill-ins: Dark mode, export functionality
  Avoid: Over-engineering, edge case optimization
```

**User Story Mapping:**

```
Backbone (Must Have):
  └─ Authentication
     ├─ Register
     ├─ Login
     └─ Password Reset

  └─ Core Feature 1
     ├─ Basic CRUD
     └─ Validation

Secondary (Should Have):
  └─ Enhanced Features
     ├─ Search/Filter
     └─ Export/Import

Tertiary (Could Have):
  └─ Delighters
     ├─ Notifications
     └─ Customization
```

### Phase 7: Implementation Roadmap

**Phased Approach:**

```
Phase 1: Foundation (Weeks 1-4)
├─ Project setup and infrastructure
├─ Authentication system
├─ Core data model
├─ Basic CRUD operations
└─ CI/CD pipeline

  Deliverables:
  ✓ Working authentication
  ✓ Basic user management
  ✓ Deployed staging environment

  Success Criteria:
  ✓ Users can register, login, logout
  ✓ Data persists correctly
  ✓ Zero critical bugs


Phase 2: Core Features (Weeks 5-8)
├─ Primary business logic
├─ Key integrations
├─ Core workflows
└─ Basic UI/UX

  Deliverables:
  ✓ Core feature complete
  ✓ End-to-end user journeys
  ✓ Performance targets met

  Success Criteria:
  ✓ All must-have features working
  ✓ <200ms response time (p95)
  ✓ 99% uptime


Phase 3: Enhancement (Weeks 9-12)
├─ Secondary features
├─ Advanced UI/UX
├─ Analytics and monitoring
└─ Performance optimization

  Deliverables:
  ✓ Should-have features complete
  ✓ Production-ready monitoring
  ✓ Performance optimized

  Success Criteria:
  ✓ <100ms response time (p95)
  ✓ Full test coverage
  ✓ Production deployment ready


Phase 4: Polish & Launch (Weeks 13-16)
├─ Bug fixes and refinements
├─ Documentation
├─ Training materials
└─ Marketing preparation

  Deliverables:
  ✓ Production deployment
  ✓ User documentation
  ✓ Support documentation

  Success Criteria:
  ✓ Zero critical bugs
  ✓ Documentation complete
  ✓ Team trained
```

**Milestone Dependencies:**

```
[Start]
  ↓
Phase 1 (Foundation) ─────┐
  ↓                       │
Phase 2 (Core) ───────────┤──→ [Launch Ready]
  ↓                       │
Phase 3 (Enhancement) ────┘
  ↓
Phase 4 (Polish)
  ↓
[Launch]
```

**Risk Mitigation:**

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Technical complexity | Medium | High | Prototype early, validate assumptions |
| Scope creep | High | Medium | Strict prioritization, change process |
| Integration issues | Medium | High | Mock external dependencies, test early |
| Performance scaling | Low | High | Load testing, design for scale |
| Team availability | Medium | Medium | Cross-train, document everything |

### Phase 8: Quality & Testing Strategy

**Testing Levels:**

```
Unit Tests
  - Individual functions and components
  - Fast, isolated, comprehensive
  - Target: >80% coverage

Integration Tests
  - API endpoints, database interactions
  - Test component communication
  - Target: Critical paths covered

End-to-End Tests
  - User workflows and journeys
  - Realistic scenarios
  - Target: Happy paths + edge cases

Performance Tests
  - Load testing, stress testing
  - Identify bottlenecks
  - Target: Meet SLA under expected load

Security Tests
  - Vulnerability scanning
  - Penetration testing
  - Target: Zero critical vulnerabilities
```

**Quality Gates:**

```
Pre-commit:
  ✓ Linting passes
  ✓ Unit tests pass
  ✓ Code reviewed

Pre-merge:
  ✓ All tests pass
  ✓ Coverage threshold met
  ✓ No new vulnerabilities
  ✓ Performance benchmarks pass

Pre-deploy:
  ✓ Integration tests pass
  ✓ E2E tests pass
  ✓ Security scan clean
  ✓ Smoke test on staging
```

### Phase 9: Launch Strategy

**Phased Rollout:**

```
1. Alpha (Internal)
   └─ Team testing
   └─ Critical bugs only

2. Beta (Limited Users)
   └─ Trusted customers/friends
   └─ Feedback collection
   └─ Bug fixes and iterations

3. Early Access
   └─ Waitlist users
   └─ Feature-complete
   └─ Monitoring intensive

4. Public Launch
   └─ General availability
   └─ Marketing push
   └─ Support ready
```

**Feature Flags:**
- Gradual rollout (1% → 10% → 50% → 100%)
- Instant rollback capability
- A/B testing support
- Environment-specific features

**Monitoring & Alerting:**

```
Metrics to Track:
  - Business: Signups, active users, conversions
  - Technical: Error rate, response time, throughput
  - Infrastructure: CPU, memory, disk, network

Alert Thresholds:
  - Error rate > 1% → Page on-call
  - P95 latency > 500ms → Warning
  - Uptime < 99.9% → Critical alert
  - Disk usage > 80% → Warning
```

### Phase 10: Post-Launch & Iteration

**Feedback Collection:**

```
Channels:
  - In-app feedback forms
  - User interviews and surveys
  - Support ticket analysis
  - Analytics and heatmaps
  - Social media monitoring

Feedback Analysis:
  - Categorize by type (bug, feature, UX)
  - Prioritize by impact and frequency
  - Identify patterns and themes
  - Close the loop with users
```

**Continuous Improvement:**

```
Iteration Cycle:
  1. Collect feedback (1-2 weeks)
  2. Analyze and prioritize (1 week)
  3. Design solutions (1-2 weeks)
  4. Develop and test (2-4 weeks)
  5. Deploy and monitor (ongoing)
  6. Measure impact (2-4 weeks)
```

**Backlog Management:**

```
Backlog Sources:
  - User feedback
  - Analytics insights
  - Technical debt
  - New opportunities
  - Competitive analysis

Prioritization Framework:
  - Impact (High/Medium/Low)
  - Effort (High/Medium/Low)
  - Strategic alignment
  - Dependencies
  - Timeline constraints
```

## Output Format

Provide a comprehensive product design document:

```
# Product Design: [Product Name]

## 1. Executive Summary
[2-3 paragraph overview of the product, its purpose, and key features]

## 2. Problem Statement
**Current Situation:**
[Describe the problem users face]

**Proposed Solution:**
[How this product solves the problem]

**Value Proposition:**
[Why users will choose this solution]

## 3. User Analysis

### Target Users
[User personas with goals, pain points, behaviors]

### User Journeys
[Key workflows and use cases]

## 4. Product Requirements

### Must-Have Features (MVP)
1. [Feature] - [Description] - [User value]

### Should-Have Features
1. [Feature] - [Description] - [User value]

### Future Enhancements
1. [Feature] - [Description] - [User value]

### Non-Functional Requirements
- Performance: [Specific metrics]
- Security: [Requirements]
- Scalability: [Expected scale]
- Accessibility: [WCAG level, etc.]

## 5. System Architecture

### High-Level Architecture
[ASCII diagram or description of components]

### Technology Stack
- **Frontend:** [Framework, UI library, build tools]
- **Backend:** [Language, framework, key libraries]
- **Database:** [Primary DB, cache, search]
- **Infrastructure:** [Hosting, CDN, monitoring]
- **Third-party:** [APIs, services, tools]

### Data Model
[Key entities and relationships]

### API Design
[Core endpoints and contracts]

## 6. User Experience Design

### Design Principles
[Core UX philosophy]

### Key Screens & Flows
[Critical user interfaces]

### Information Architecture
[Navigation structure]

### Accessibility
[How accessibility is addressed]

## 7. Implementation Plan

### Phase 1: Foundation [Timeline]
**Goals:** [What will be achieved]
**Tasks:**
- [ ] [Task 1]
- [ ] [Task 2]

**Deliverables:** [Concrete outputs]
**Success Criteria:** [Measurable outcomes]

### Phase 2: Core Features [Timeline]
[Same structure]

### Phase 3: Enhancement [Timeline]
[Same structure]

### Phase 4: Launch [Timeline]
[Same structure]

## 8. Risk Assessment

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| [Risk 1] | [High/Med/Low] | [High/Med/Low] | [How to address] |
| [Risk 2] | [High/Med/Low] | [High/Med/Low] | [How to address] |

## 9. Success Metrics

### Business Metrics
- [Metric 1]: [Target value]
- [Metric 2]: [Target value]

### User Metrics
- [Metric 1]: [Target value]
- [Metric 2]: [Target value]

### Technical Metrics
- [Metric 1]: [Target value]
- [Metric 2]: [Target value]

### Quality Metrics
- [Metric 1]: [Target value]
- [Metric 2]: [Target value]

## 10. Launch Strategy

### Phased Rollout
1. **Alpha:** [Who, what, when]
2. **Beta:** [Who, what, when]
3. **Early Access:** [Who, what, when]
4. **Public Launch:** [When, how]

### Go/No-Go Criteria
- [ ] All critical bugs resolved
- [ ] Performance benchmarks met
- [ ] Security audit passed
- [ ] Documentation complete
- [ ] Support team trained

## 11. Post-Launch Plan

### Monitoring & Maintenance
- [What will be monitored]
- [Maintenance schedule]
- [On-call procedures]

### Feedback Collection
- [How feedback will be gathered]
- [How it will be used]

### Continuous Improvement
- [Iteration cadence]
- [Backlog management]
- [Feature prioritization]

## 12. Next Steps

1. [Immediate action item]
2. [Immediate action item]
3. [Immediate action item]

---

**Appendix: Additional Resources**

- [Competitive Analysis]
- [Technical References]
- [Design Mockups (if applicable)]
- [Research Data]
```

## Design Principles

When creating product designs:

1. **Start with users** - Every feature should solve a real user problem
2. **Think MVP first** - Ship the smallest thing that delivers value
3. **Iterate quickly** - Get user feedback early and often
4. **Measure everything** - Data should inform decisions
5. **Balance trade-offs** - Speed vs. quality, features vs. simplicity
6. **Plan for scale** - Architecture should support growth
7. **Security by design** - Build security in from the start
8. **Accessibility first** - Design for everyone
9. **Technical feasibility** - Realistic about what can be built
10. **Business alignment** - Product decisions support business goals

## Common Product Design Patterns

### SaaS Application
```
Core: Multi-tenant architecture
Auth: OAuth 2.0, SSO
Billing: Subscription management
Admin: Role-based access control
Analytics: Usage tracking and reporting
```

### Mobile App
```
Core: Offline-first architecture
Sync: Background data synchronization
Storage: Local-first, cloud backup
Push: Real-time notifications
Auth: Biometric + OAuth
```

### API/Platform
```
Core: RESTful or GraphQL design
Docs: Interactive documentation
Auth: API keys, OAuth 2.0
Rate limiting: Tier-based access
Monitoring: API usage analytics
```

### E-Commerce
```
Core: Product catalog and cart
Payments: Stripe/PayPal integration
Inventory: Stock management
Search: Elasticsearch or similar
Recommendations: ML-based suggestions
```

### Content Platform
```
Core: CMS for content management
Media: Image/video optimization
Search: Full-text search
Personalization: User preferences
Analytics: Content performance
```

## When to Use This Skill

Invoke this skill when:
- Planning a new product or feature
- Designing system architecture
- Creating technical specifications
- Planning implementation roadmaps
- Evaluating technology options
- Defining MVP scope
- Creating launch strategies
- Planning product iterations

This skill transforms requirements into actionable, comprehensive product designs that balance user needs, technical feasibility, and business goals.
