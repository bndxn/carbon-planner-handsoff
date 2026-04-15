# Security

Sources:
* OWASP Top 10 (https://owasp.org/Top10/2025/)
* https://www.ncsc.gov.uk/cyberessentials/overview
* https://ce-knowledge-hub.iasme.co.uk/space/CEKH/2650538068/Securing+your+cloud+services
* MITRE ATT&CK : https://attack.mitre.org/

This project must meet the following baseline controls.

## 1) Accurate asset inventories

All AWS resources (S3 buckets, CloudFront distributions, API Gateway, Lambda, EventBridge, DynamoDB, IAM roles/policies, KMS keys, alarms) must be declared in infrastructure as code.

Requirements:

- It must be possible to recreate the full stack in a fresh AWS account with environment variables only (for example account ID, region, environment name).
- No manually created "hidden" runtime dependencies.
- Maintain a comprehensive AWS resource inventory in `README.md`, with brief descriptions for human review.

## 2) Robust access controls

IAM must follow least privilege by default.

Requirements:

- Separate execution roles per component (e.g., API Lambda vs scheduled job Lambda).
- Policies must grant only required actions on specific resources; avoid `*` resources/actions unless justified and documented.
- Any role assumption paths must be documented, including trust policies.
- Human access should use short-lived credentials and MFA.

## 3) Secure configuration

Secure-by-default cloud configuration is required.

Requirements:

- S3 buckets: block public access unless explicitly needed and reviewed.
- Encryption at rest enabled (S3, DynamoDB, logs where configurable).
- TLS enforced for all client-facing traffic.
- Secrets stored in AWS Secrets Manager or SSM Parameter Store (never in code or plain env files committed to git).
- Production and development environments must be logically separated.

## 4) Comprehensive logging and detection

Logging must support investigation and operational monitoring.

Requirements:

- CloudTrail enabled for account-level API activity.
- CloudWatch logs for Lambda/API with retention period configured.
- Log key security-relevant events: auth failures, permission denials, unexpected external API failures, data refresh failures.
- Create alarms for repeated failures and stale recommendation data.
- Include request IDs/correlation IDs for traceability.

## 5) Vulnerability and update management

Dependency and configuration drift must be monitored continuously.

Requirements:

- Enable Dependabot (or equivalent) for dependency updates.
- Run dependency vulnerability scanning in CI.
- Run static checks in CI (`ruff`, tests, and any security linters adopted).
- Patch critical vulnerabilities with priority and track remediation in PRs/issues.
- Auto-merge is allowed only for low-risk updates after all required checks pass.

## 6) Data handling and privacy

This project is low-PII by design; maintain that property.

Requirements:

- Do not collect personal user data unless explicitly required by a future feature.
- If telemetry is added, aggregate/anonymize where possible.
- Never log secrets, tokens, or sensitive headers.

## 7) Supply-chain and CI/CD safeguards

- Protect main branch with required status checks.
- Require pull requests; no direct pushes to protected branches.
- Use pinned actions/versions in CI where practical.
- Restrict deployment credentials to the minimum required scope.