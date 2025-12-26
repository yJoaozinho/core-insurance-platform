# ADR-001: Architectural Style – Modular Monolith

## Status
Accepted

## Context

The goal of this project is to design a **core insurance platform** for a large-scale insurance domain, focusing on:
- Domain-driven design (DDD)
- Clear separation of concerns
- Long-term maintainability and evolvability

The system includes complex business domains such as:
- Policy Management
- Claims Management
- Billing
- Risk and Pricing

At this stage of the project, the primary objective is **accurate domain modeling and architectural clarity**, rather than premature scalability optimization.

Given the complexity of the insurance domain and the early phase of the system, introducing distributed architecture upfront would significantly increase:
- Operational complexity
- Debugging difficulty
- Transactional and consistency challenges

## Decision

The system will be implemented as a **Modular Monolith**, structured around well-defined **bounded contexts**.

Each bounded context:
- Has clear domain boundaries
- Owns its domain logic
- Communicates with other contexts through explicit interfaces
- Can evolve independently within the same codebase

This approach allows the system to maintain:
- Strong domain isolation
- Simpler deployment and operational model
- Easier reasoning about business rules and workflows

## Consequences

### Positive
- Enables deep focus on domain modeling and DDD practices
- Reduces architectural and operational complexity in early stages
- Simplifies transaction management and data consistency
- Improves developer productivity and system comprehensibility

### Negative
- Independent horizontal scaling per bounded context is limited
- Fault isolation is weaker compared to distributed architectures

### Future Evolution

This architectural style **does not prevent** future evolution toward microservices.

If scaling, organizational structure, or operational requirements change, individual bounded contexts can be:
- Extracted into independent services
- Deployed separately
- Scaled and evolved autonomously

The modular monolith acts as a **foundation for safe and intentional service decomposition**.

## Alternatives Considered

### Microservices Architecture
- Rejected at this stage due to:
  - Increased operational complexity
  - Higher cognitive load
  - Premature optimization before domain stabilization

### Fully Layered Monolith Without Domain Boundaries
- Rejected due to:
  - Weak domain isolation
  - Higher risk of tight coupling
  - Reduced long-term maintainability

## Notes

This decision prioritizes **clarity, correctness, and domain integrity** over early distribution, aligning with real-world practices in complex enterprise systems.
