# Architecture Overview

This document defines the strict custom architecture, conventions, and separation of concerns for this project. All code changes must align with the structure and practices below.

---

## 1. Code Generation & Organization

- Always create and reference files in the correct directory according to their function.
- Maintain strict separation between frontend, backend, and shared code.
- Adopt the technologies below and keep layering boundaries clean.

### Technologies
- Frontend: React/Next.js (TypeScript)
- Backend: Node.js/Express (TypeScript)
- Shared: TypeScript types, DTOs, and utilities

### Directory Structure
```
/frontend/                     # UI and client app (Next.js)
  src/
    components/               # Reusable UI components
    pages/                    # Next.js pages/routes
    features/                 # Feature modules (hooks, state, services)
    services/                 # API clients (consume backend endpoints)
  public/                     # Static assets

/backend/                      # Server app (Express)
  src/
    api/                      # Controllers/route handlers
    services/                 # Business logic
    models/                   # Data models (ORM schemas)
    middleware/               # Auth, validation, error handling
    config/                   # Environment/configuration

/common/                       # Shared code strictly without runtime deps
  types/                      # Shared TypeScript types and DTOs
  utils/                      # Side-effect free utilities

/tests/                        # Top-level test artifacts (if not colocated)
  frontend/                   # Frontend tests (Jest)
  backend/                    # Backend tests (Jest)

/scripts/                      # Dev and CI helper scripts
/.github/workflows/            # CI/CD pipelines
```

> Example references: use `/backend/src/api/` for controllers, `/frontend/src/components/` for UI, `/common/types/` for shared models.

---

## 2. Context-Aware Development

- Before generating or modifying code, read the relevant section of this document for alignment.
- Infer dependencies and interactions between layers explicitly:
  - Frontend services consume backend API endpoints using typed clients.
  - All request/response contracts use DTOs defined in `/common/types/`.
- When introducing new features, document where they fit in the architecture and why.
- Follow clean boundaries: UI → services → API; API → controllers → services → models.

---

## 3. Documentation & Scalability

- Update `ARCHITECTURE.md` whenever structural or technological changes occur.
- Generate docstrings, type definitions, and comments automatically where possible (e.g., TSDoc, typed DTOs).
- Naming conventions:
  - Files: `kebab-case.ts` for non-components; Components: `PascalCase.tsx`.
  - Functions/variables: `camelCase`; Types/Interfaces: `PascalCase`.
- Module boundaries:
  - Feature modules encapsulate state, hooks, and services.
  - Shared code (`/common`) must be framework-agnostic and side-effect free.

---

## 4. Testing & Quality

- Generate matching test files under `/tests/` or colocate with modules (e.g., `Component.test.tsx`).
- Frameworks & tooling: Jest, Testing Library (frontend), ESLint, Prettier, TypeScript strict mode.
- Maintain strict TypeScript coverage and linting standards; CI fails on type or lint errors.
- Suggested config:
  - Frontend: React Testing Library, MSW for API mocking.
  - Backend: Supertest for HTTP, unit tests for services.

---

## 5. Security & Reliability

- Implement secure authentication (JWT or OAuth2) and data protection practices (TLS, AES-256 for at-rest where applicable).
- Include robust error handling, input validation, and logging:
  - Validation: Zod or Joi schemas on all inbound requests.
  - Error handling: centralized Express error middleware returning typed error responses.
  - Logging: structured logger (pino/winston) with request correlation IDs.
- Secrets management: use environment variables; never commit secrets.

---

## 6. Infrastructure & Deployment

- Generate infrastructure files according to `/scripts/` and `/.github/` conventions.
- CI/CD:
  - Lint, type-check, test, build steps must all pass.
  - Artifacts: frontend build (`.next`), backend bundle (e.g., `dist/`).
- Deployment targets (examples):
  - Frontend: Vercel or Netlify
  - Backend: Render/Fly.io/Dokku; attach managed DB (PostgreSQL)
- Configuration:
  - Env files: `.env.local` (dev), `.env` (CI), managed secrets in hosting provider.

---

## 7. Roadmap Integration

- Annotate potential technical debt or optimizations directly in this document for future developers.
- For each feature, link to product requirements and describe architectural placement and trade-offs.

---

## Current Repository Notes

This repo currently contains a static `index.html` and image assets. To align with the architecture above, the recommended path is:

1. Initialize `/frontend/` as a Next.js TypeScript app; move static images to `/frontend/public/`.
2. Create `/common/types/` for any shared models used by both layers.
3. Optionally scaffold `/backend/` (Express + TypeScript) when server-side features are needed.
4. Add CI workflows to lint, type-check, test, and build on push/pull requests.

No immediate code movement has been performed to preserve the current site; the document establishes conventions for future development.