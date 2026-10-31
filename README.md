# Terravigor Telemetry Service

**Terravigor · Industrial Telemetry**

> Independently deployable domain service in the Terravigor intelligent mining fleet operations platform.

<sub>Repository: `terravigor-telemetry-service` · Delivery sequence: 3 of 8 · Initial status: architecture scaffold</sub>

## Service mission

Owns ingestion and validation of GPS, speed, fuel, engine temperature, payload and device timestamps; deduplication, latest-state projections, retention and telemetry query APIs.

## Bounded context

**Owns:** telemetry_db and the domain data described above.

**Does not own:** Does not own equipment master data or maintenance work orders. Do not make Redis the source of truth for telemetry.

## Primary persistence

`telemetry_db`

Each service owns its own database, schema, credentials and Flyway migrations. A local developer database server may host isolated logical databases for convenience, but no service may access another service's schema.

## Integration boundaries

**Expected dependencies:**
- Fleet Service equipment/device contract
- Kafka
- PostgreSQL/time-series strategy

**Candidate domain events (contracts must be reviewed before implementation):**
- `TelemetryAccepted`
- `TelemetryRejected`
- `EquipmentStateUpdated`
- `TelemetryAnomalyDetected`

REST is for request/response interactions that require an immediate result. Kafka is for durable asynchronous domain events. Consumers must be idempotent. Use the Transactional Outbox pattern when a database state change and event publication must be atomic.

## Technology baseline

- Java 25 LTS; no preview language features.
- Spring Boot 4.1.x, with exact patch locked after compatibility verification.
- Spring Framework / Security versions managed by the Spring Boot BOM.
- Spring Data JPA + Hibernate for relational persistence.
- PostgreSQL and Flyway migrations.
- Spring for Apache Kafka where event messaging is needed.
- Micrometer / OpenTelemetry for observability.
- Maven Wrapper for reproducible builds.
- Docker multi-stage images and Kubernetes/Helm deployment.

Only add libraries required by this service. Do not copy every platform dependency into every microservice.

## Required implementation deliverables

This repository is intentionally not populated with business code yet. When its implementation phase is authorized, the service must deliver:

- [ ] Reviewed OpenAPI contract and error model.
- [ ] Domain/application/API/infrastructure code.
- [ ] PostgreSQL schema, Flyway migrations, constraints and workload-driven indexes.
- [ ] Unit, integration, authorization and contract tests.
- [ ] `Dockerfile` (multi-stage build, non-root runtime) and `.dockerignore`.
- [ ] Local Docker Compose integration with health checks.
- [ ] Kubernetes Deployment and Service, probes, resource requests/limits, security context and Secret references.
- [ ] Helm chart with validated values and no secret values.
- [ ] CI build/test/security/container/manifest validation pipeline.
- [ ] Actuator health, metrics, structured logs and distributed tracing.
- [ ] Service README, runbook, failure modes and rollback instructions.

Do not commit non-functional placeholder manifests. Add executable deployment files with the actual application configuration during this service's implementation phase.

## Development protocol

Follow `AGENTS.md`. Build services strictly in the approved order. The first implementation target is Identity Service. This repository's code phase begins only after earlier delivery gates are approved.

## Repository standards

See `CONTRIBUTING.md`, `SECURITY.md`, `.github/` templates and `docs/`. No open-source license has been selected; all rights are reserved until a license is deliberately added.
