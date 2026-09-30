# Architecture

## Why a polyrepo with a shared schema

The GraphQL schema is published as a versioned package. Both consumers pin a version:

- **Backend** imports `typeDefs` from the package and builds an executable schema with resolvers.
- **Frontend** points `graphql-codegen` at the package's `schema.graphql` and generates a
  fully typed `graphql()` document function used by urql.

A breaking schema change ships as a new release; consumers upgrade deliberately. CI in the
consumers fails fast if an operation no longer matches the contract (codegen / type-check).

## Local development

```bash
# backend
cd restaurant-delivery-backend && NODE_AUTH_TOKEN=<pkg-read-token> npm ci && npm start
# frontend
cd restaurant-delivery-frontend && NODE_AUTH_TOKEN=<pkg-read-token> \
  VITE_API_URL=http://localhost:4000/graphql npm run dev
```

## Release process

1. Edit `schema.graphql`, bump version, open a PR, merge to `main`.
2. Cut a GitHub Release → `Publish` workflow pushes the npm package + attaches the SDL.
3. Bump the dependency in backend/frontend, open PRs, merge → Render auto-deploys.
