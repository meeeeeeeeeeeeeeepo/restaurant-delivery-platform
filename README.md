# 🍝 Restaurant Delivery Platform

A polyrepo demonstrating a **shared GraphQL contract** consumed by both a frontend and a
backend, with CI, published artifacts, Slack automation, and Render deployments.

## Repositories

| Repo | Role | Stack | Deploy |
|---|---|---|---|
| [restaurant-graphql-schema](https://github.com/meeeeeeeeeeeeeeepo/restaurant-graphql-schema) | Source of truth — GraphQL SDL, published as an artifact | GraphQL SDL + npm package | GitHub Packages + Release |
| [restaurant-delivery-backend](https://github.com/meeeeeeeeeeeeeeepo/restaurant-delivery-backend) | GraphQL API implementing the schema | graphql-yoga (Node) | Render web service |
| [restaurant-delivery-frontend](https://github.com/meeeeeeeeeeeeeeepo/restaurant-delivery-frontend) | Storefront; typed ops generated from the schema | React + Vite + urql | Render static site |

## Contract flow

```
restaurant-graphql-schema  ──(publish @meeeeeeeeeeeeeeepo/restaurant-schema)──┐
          │ SDL                                                                │
          ▼                                                                    ▼
   backend: graphql-yoga implements it            frontend: graphql-codegen generates typed hooks
          │                                                                    │
          ▼                                                                    ▼
   Render web service  ◄───────────── GraphQL /graphql ─────────────  Render static site
```

The schema is the only coupling point: the backend **implements** it and the frontend
**generates typed operations** from it, so neither side can drift from the API.

## Published artifacts

- `@meeeeeeeeeeeeeeepo/restaurant-schema` on **GitHub Packages** (npm), versioned per release.
- `schema.graphql` attached to each schema **GitHub Release**.
- Per-CI build artifacts (`schema SDL`, `backend-src`, `frontend-dist`) via `actions/upload-artifact`.

## Environments

Each service deploys to Render with **production** (`main`) and **staging** (`develop`) targets,
defined in each repo's `render.yaml`.

## CI/CD & automation

- GitHub Actions in every repo: lint, test, build, publish, upload artifacts.
- Slack: the GitHub app posts PR/commit/CI activity into the workspace; a Slack Workflow
  automates release announcements; a Slack Code channel is used for AI-assisted changes.

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).
