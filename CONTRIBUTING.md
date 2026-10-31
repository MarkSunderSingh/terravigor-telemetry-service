# Contributing

## Workflow
1. Open or identify an issue that describes the change.
2. Create a focused branch from `main` (for example, `feat/add-equipment-registration`).
3. Keep the change scoped to this service and its bounded context.
4. Add or update unit, integration and contract tests.
5. Run the relevant checks locally and include exact commands/results in the pull request.
6. Open a pull request using the repository template.

## Commit messages
Use concise Conventional Commit-style messages:
- `feat: add equipment registration`
- `fix: reject duplicate telemetry event`
- `docs: clarify service ownership`
- `test: cover tenant authorization`
- `build: pin runtime image`

## Engineering expectations
- No secrets, credentials, tokens or real operational/customer data in commits.
- Do not read or update another service's database.
- Changes to public REST/event contracts require compatibility review.
- Every remote call must have explicit timeouts; retries must be bounded and safe.
- Do not claim tests, performance or security checks passed unless they ran.
