# Architecture

## Principles

- Serverless-first and low-ops
- Lowest practical cost for hobby/personal use
- Clear separation between ingestion, scoring, and presentation
- Deterministic core logic (easy to test and reason about)

## High-level design

### Frontend

- Static web app (recommended: React + Vite or Next.js static output)
- Hosted on S3 + CloudFront (or Amplify if preferred for speed)
- Reads recommendations from a backend API only (no direct third-party API calls)

### Backend compute

- Scheduled AWS Lambda (`planner_job`) runs every 3 hours via EventBridge
- Optional on-demand Lambda invocation endpoint for manual refresh (authenticated admin-only)
- Lambda tasks:
  1. Fetch carbon intensity forecast for London
  2. Fetch weather forecast for London
  3. Normalize data into common time buckets (30-minute intervals)
  4. Compute ranked recommendation windows
  5. Write results to storage

### Data/API layer

- DynamoDB table for latest recommendation snapshots (partition key by drying mode)
- API Gateway + Lambda (`planner_api`) for frontend reads:
  - `GET /recommendations?mode=indoor`
  - `GET /recommendations?mode=outdoor`
- API returns payload with:
  - recommended windows
  - scoring breakdown
  - source timestamps
  - generated timestamp

### Observability

- CloudWatch structured logs for all Lambda executions
- CloudWatch metric filters/alarms on:
  - Lambda errors
  - stale data threshold breach (e.g., data older than 6 hours)
  - API 5xx rate

## Data contracts

## External input model (normalized)

- `timestamp_utc` (ISO8601)
- `carbon_intensity_gco2_per_kwh` (number)
- `temperature_c` (number)
- `precip_probability_pct` (number)
- `wind_speed_mps` (number)
- `humidity_pct` (number)

## Recommendation output model

- `mode`: `indoor | outdoor`
- `generated_at_utc`: ISO8601
- `valid_from_utc`: ISO8601
- `valid_to_utc`: ISO8601
- `windows`: array of:
  - `start_utc`
  - `end_utc`
  - `overall_score` (0-100)
  - `carbon_score` (0-100)
  - `drying_score` (0-100, for outdoor; fixed baseline for indoor)
  - `explanation` (short human-readable reason)

## Scoring strategy (MVP)

- Indoor mode:
  - prioritize low carbon intensity
  - optional minor penalty for extreme humidity
- Outdoor mode:
  - weighted composite of carbon + dry-weather suitability
  - weather penalties for high precipitation probability and high humidity
- Keep score computation pure and deterministic in a dedicated module
- Include constants and weights in one config file to make tuning easy

## Deployment and environments

- Environments: `dev` and `prod`
- Infrastructure as code required (Terraform or AWS CDK; choose one and document why)
- One-command deployment for each environment
- No manual console-only resources

## Future extension points

- Agentic reasoning layer that explains trade-offs in natural language
- User preferences (quiet hours, preferred days)
- Notifications (email/push) when an optimal window is upcoming
