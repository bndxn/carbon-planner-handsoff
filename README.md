# Carbon Planner (Hands-off)

Carbon Planner is a low-cost, serverless web app that recommends the best upcoming times to run a laundry cycle in London, balancing:

- UK grid carbon intensity (lower is better)
- Weather suitability for line drying (if drying outdoors)

The app should support two drying modes:

- **Indoor drying**: optimize mostly for grid carbon intensity
- **Outdoor line drying**: optimize for both grid carbon intensity and weather suitability

## Project goals

- Provide practical recommendations for the next 24-72 hours
- Keep cloud cost and operational overhead low
- Be simple to run and maintain by one person
- Use deterministic scoring logic first (agentic enhancement can be layered on later)

## Product scope (MVP)

### User journey

1. User opens web app
2. User chooses drying mode (`indoor` or `outdoor`)
3. User sees ranked recommended time windows
4. User can inspect "why this is recommended" (carbon + weather breakdown)

### Functional requirements

- Show recommendations in local London time
- Provide at least top 3 windows
- Refresh recommendations at least every 3 hours
- Cache results so frontend does not call external APIs directly
- Expose recommendation confidence and data freshness timestamp

### Non-functional requirements

- Serverless architecture on AWS
- Robust defaults with clear error handling and observability
- Fast frontend load (<2s on warm path target)
- Easy local development and repeatable deployment

## Specs and constraints

- Architecture and component responsibilities: `ARCHITECTURE.md`
- Coding and quality standards: `SOFTWARE_STANDARDS.md`
- Security baseline and controls: `SECURITY.md`

## Definition of done (MVP)

- Infrastructure deploys from scratch into a new AWS account
- Scheduled backend job ingests required data and stores computed recommendations
- Frontend displays ranked recommendations with explanation metadata
- CI runs linting, tests, and coverage gates
- Documentation is sufficient for another engineer/agent to extend safely