# AI Coding Agent Rules — Telemetry Service

Read this file and the service README before changing anything. Read the Terravigor project architecture and service contract decisions before implementing business code.

## Authorized scope
This repository is for **Telemetry Service only**. Do not create or modify other microservices from this repository.

Delivery sequence: 3 of 8. Do not begin this service until the project owner has approved the preceding service's exit gate.

## Mandatory workflow
1. Inspect the repository and current branch.
2. Read service ownership, dependencies and out-of-scope responsibilities in README.
3. Present a short plan listing files, dependencies, APIs/events, migrations, tests and acceptance criteria.
4. Confirm exact Java/Spring/Spring Cloud/Spring AI version compatibility from official release documentation where relevant.
5. Define and review API/event contracts before implementing integration.
6. Implement one vertical slice at a time; do not generate an entire service in one pass.
7. Add tests and execute them. Report exact commands and honest results.
8. Update documentation, ADRs and implementation status with verified information.
9. Stop at the phase gate and wait for owner approval.

## Non-negotiable rules
- Java 25 LTS, Maven Wrapper, pinned dependencies and no preview features.
- Package by domain/feature; keep domain, application, API and infrastructure responsibilities clear.
- Use constructor injection, DTOs, Bean Validation and RFC 9457 Problem Details.
- Never expose JPA entities directly from REST controllers.
- PostgreSQL and Flyway own schema changes; do not use `ddl-auto=update`.
- This service owns `telemetry_db`. Never query or mutate another service's database.
- Use UTC timestamps and `java.time`. Use BigDecimal for precise decimal quantities where appropriate.
- Add explicit HTTP timeouts. Retries must be bounded and safe. Do not fake success in write fallbacks.
- Kafka events need stable IDs, versioned schemas, correlation IDs and idempotent consumers.
- Do not dual-write database + Kafka. Use an outbox when atomic publication is required.
- Enforce authorization and tenant/object ownership within this service, not only at the gateway.
- Never commit secrets, credentials, personal data or real mine telemetry.
- Do not add dependencies, endpoints, event topics or services without a clear requirement and approval.
- Do not claim performance, security, tests or deployments succeeded unless measured/executed.

## Required quality gates
Unit tests; PostgreSQL Testcontainers integration tests; API/security/tenant tests; event contract and consumer idempotency tests where applicable; Docker build; non-root image validation; Kubernetes schema validation; Helm lint/template; CI checks; operational documentation.

## Naming
- Repository/service names: kebab-case.
- Java package: `com.terravigor.telemetry`.
- Java classes: PascalCase; methods/fields: lowerCamelCase; constants: UPPER_SNAKE_CASE.
- SQL identifiers: snake_case.
- REST paths: versioned plural nouns under `/api/v1`.
- Kafka topics: `terravigor.<domain>.<event-name>.v<major>`.

## Stop conditions
Stop and ask the project owner when a security boundary, data owner, API contract, event compatibility decision or framework compatibility issue is unclear.
