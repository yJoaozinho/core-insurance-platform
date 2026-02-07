# Core Insurance Platform – Technical Project

## 1. Overview

This project represents the initial design and implementation of a core insurance platform, created as part of a technical evaluation process.

The main objective of this system is not to deliver a feature-complete insurance product, but to demonstrate the ability to reason about, model, and design complex software systems under real-world constraints commonly found in large-scale insurance environments.

The platform is designed around fundamental software engineering principles such as domain-driven design (DDD), clear separation of concerns, and long-term maintainability. Special attention is given to domain modeling, architectural decisions, and trade-offs related to scalability, consistency, auditability, and regulatory requirements.

Rather than focusing on user interfaces or superficial functionality, this project emphasizes the internal structure of the system: how core business concepts are represented, how responsibilities are divided across layers, and how the system can evolve safely over time as business rules and regulations change.

This repository documents the current progress of the project, including architectural documentation, domain analysis, design decisions, and initial technical foundations.

## How to navigate this repository

### Repository Navigation

- [`architecture/`](architecture/)
- [`progress/`](progress/)

This repository contains:

- `architecture/architecture.md`: detailed architectural overview
- `architecture/adrs/`: architectural decision records
- `architecture/diagrams/`: system, context, bounded context, and component diagrams
- `domain/`: domain definitions and modeling notes


## 2. Problem Statement

Insurance core systems operate in highly complex and regulated environments. They must handle long-lived business processes, evolving rules, strict auditability requirements, and high expectations around reliability and data consistency. The challenge addressed by this project is to design a core insurance platform capable of supporting essential insurance operations—such as policy lifecycle management, claims processing, and billing—while remaining scalable, maintainable, and adaptable to change over time.

## 3. Domain Understanding

The insurance domain is centered around managing risk over time through contractual agreements between an insurer and a customer. From a software perspective, the domain is characterized by long-lived entities, strict historical consistency, and complex business rules that evolve over time.

This project focuses on the core concepts commonly found in non-life insurance systems, abstracted to a level suitable for architectural and domain modeling purposes.

### Key Domain Concepts

- **Policy**  
  A Policy represents the contractual agreement between the insurer and the insured party. It defines what is covered, under which conditions, and during which time period. Policies are long-lived entities and may change over time through endorsements, renewals, or cancellations, while preserving historical versions.

- **Coverage**  
  Coverage defines specific risks or events that are protected under a policy. A policy may contain multiple coverages, each with its own limits, conditions, and exclusions.

- **Premium**  
  The Premium is the price paid by the customer in exchange for the coverage provided by the policy. Premium calculation is typically based on risk analysis and pricing rules.

- **Claim**  
  A Claim represents a request for compensation made by the insured after the occurrence of an insured event (loss). Claims follow a lifecycle with multiple states and validations focused on eligibility and policy compliance.

- **Risk**  
  Risk represents the exposure evaluated by the insurer when pricing and issuing a policy. Risk assessment influences coverage availability, pricing, and acceptance decisions.

- **Billing**  
  Billing is responsible for managing financial obligations related to policies, including invoicing, payment schedules, and reconciliation. In the current phase, payment execution is delegated to an external payment provider.

- **Payment**  
  Payment represents the settlement of issued invoices through an external payment gateway. The platform integrates with **Stripe** to handle payment processing, while maintaining internal records for financial traceability.

- **Endorsement**  
  An Endorsement represents a modification to an active policy, such as adding or removing coverages or changing insured information, without invalidating historical data.

- **Audit Log**  
  Audit logs record all critical operations and state changes within the system to ensure traceability, compliance, and regulatory auditability.

### Domain Characteristics

- Policies and claims are **stateful** and evolve over time.
- Historical data must remain **immutable** once recorded.
- Business rules are **complex and subject to change**.
- Regulatory and compliance requirements influence data modeling and system behavior.
- Fraud detection is intentionally excluded from the current domain scope, as the insurer covers all claim types in this phase.

This domain understanding serves as the foundation for the system’s bounded contexts, aggregates, and architectural decisions described in later sections.

## 4. Functional Scope

The functional scope of this project focuses on the core capabilities expected from a foundational insurance platform. The intent is to model essential business processes with sufficient depth to support complex rules, traceability, and future evolution.

### In-Scope Functionalities

- **Policy Management**
  - Policy creation and issuance
  - Policy versioning to preserve historical consistency
  - Endorsements (policy modifications)
  - Policy renewal and cancellation workflows
  - State transitions with validation rules

- **Claims Management**
  - Claim registration linked to a valid policy
  - Claim lifecycle management (submission, validation, approval, rejection, settlement)
  - Eligibility and policy compliance validations
  - Traceability of claim decisions and state changes

- **Risk and Pricing**
  - Abstract risk evaluation model
  - Configurable pricing and rating rules
  - Separation between risk assessment and premium calculation logic

- **Billing and Payments**
  - Premium invoicing
  - Payment schedule management
  - Billing state tracking and reconciliation concepts
  - Integration with **Stripe** for payment processing

- **Auditability and Compliance**
  - Audit logging of critical operations
  - Immutable historical records for policies and claims
  - Support for regulatory traceability and inspections

- **Access Control**
  - Role-based access control (RBAC)
  - Fine-grained permissions for sensitive operations

### Out-of-Scope (Current Phase)

- User interface implementation
- Advanced fraud detection or fraud scoring systems
- External fraud analysis services
- Reporting and analytics dashboards
- Full regulatory implementation for specific jurisdictions

These elements are considered future extensions and do not impact the validity of the core architectural and domain modeling decisions.

## 5. Non-Functional Requirements

The system is designed to operate in a mission-critical environment where data consistency, traceability, and reliability are fundamental.

### Scalability
- Support growth in policies, claims, and transactions
- Allow horizontal scaling where appropriate

### Reliability and Availability
- Remain operational under partial failures
- Resilient critical operations

### Data Consistency and Integrity
- Strong consistency for core domain operations
- Immutable historical data
- Concurrency control for state transitions

### Security
- Role-based access control (RBAC)
- Least-privilege data access

### Auditability and Compliance
- Immutable audit logs
- Reconstructable historical states

### Observability
- Logging and tracing for debugging and auditing

### Maintainability and Evolution
- Business rules isolated from infrastructure
- Safe incremental evolution

## 6. Architectural Overview

The system follows a layered architecture aligned with DDD principles.

### Layers

- **Domain Layer**
  - Core business logic and rules
  - Framework-agnostic

- **Application Layer**
  - Use case orchestration
  - Transactions and authorization

- **Infrastructure Layer**
  - Persistence, security, logging
  - External integrations (e.g., Stripe)

### System Boundaries

Initially implemented as a modular monolith with clear bounded contexts, allowing future decomposition if required.

## 7. Domain-Driven Design (DDD)

DDD is used to align software models with business language and complexity.

### Bounded Contexts

- **Policy Management**
- **Claims Management** (fraud intentionally excluded)
- **Billing** (payment execution delegated to Stripe)
- **Risk & Pricing**

### Aggregates

- Policy Aggregate
- Claim Aggregate
- Billing Aggregate

## 8. Data Modeling

### Core Entities

#### Policy
- Identifier
- Status
- Effective period
- Active version reference

#### Policy Version
- Immutable
- Coverage and premium snapshot

#### Claim
- Linked to policy version
- Lifecycle states

#### Invoice
- Financial obligation
- Linked to policy version

#### Payment Record
- External reference (Stripe)
- Internal reconciliation

#### Audit Log
- Immutable event history

## 9. Technology Decisions

### Language
- **Java**

### Framework
- **Spring Boot**

### Persistence
- **Relational Database (PostgreSQL)**

### Payments
- **Stripe** via infrastructure adapters

### Security
- Spring Security
- RBAC

### Documentation
- Markdown
- C4-style diagrams

### Testing
- Domain-focused unit tests
- Workflow-oriented application tests
