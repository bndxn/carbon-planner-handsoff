# Software standards

## Engineering principles

- Prefer the simplest working solution with clear ownership boundaries
- Keep modules small and composable; avoid premature abstraction
- Remove dead code and redundant paths during each change
- Favor explicit behavior over magic side effects

## Language and project structure

- Backend language: Python 3.12+
- Organize backend by responsibility:
  - `clients/` (external API integrations)
  - `domain/` (scoring rules and pure logic)
  - `handlers/` (Lambda entry points)
  - `storage/` (DynamoDB persistence)
  - `api/` (request/response models)
- Frontend can use TypeScript; keep API contracts versioned and explicit

## Style and linting

- Use `ruff format` for formatting
- Use `ruff check` for linting
- Fail CI on lint errors
- Use Google-style Python docstrings for public modules/functions

## Testing and quality gates

- Test runner: `pytest`
- Minimum backend unit test coverage: 70%
- Required tests:
  - scoring logic deterministic behavior
  - API serialization and validation
  - external client error handling/fallback behavior
  - storage read/write integration (mocked or local Dynamo)
- CI must block merges when tests or coverage gates fail

## Error handling and resilience

- External API failures must not crash the whole pipeline silently
- Return actionable error messages with stable error codes
- Log exceptions with request correlation identifiers
- Use retries with bounded backoff for transient upstream failures

## Observability conventions

- Structured JSON logs for backend services
- Include: `timestamp`, `service`, `operation`, `level`, `request_id`
- No secrets or personal data in logs

## Dependency and change management

- Pin direct dependencies with compatible version ranges
- Use Dependabot (or equivalent) for update PRs
- All changes must pass CI before merge
- Keep PRs focused and small where possible