# ADR-001: Selection of Core Technology Stack for FitFlow

- **Status:** Accepted
- **Date:** 2026-09-19
- **Deciders:** Yasintha Wanasundara (IT23548978)
- **Supersedes:** —
- **Related:** ADR-002 (AI Service Boundaries — pending), ADR-003 (Auth Provider — pending)

---

## 1. Context

The FitFlow redesign requires a modern, cross-platform fitness application supporting:

- **iOS, Android, and Web** from a single codebase
- **High-performance** interactions (real-time workout tracking, animations)
- **AI/ML-driven personalization** (workout plans, nutrition analysis)
- **Real-time features** (live workouts, social feed, chat)
- **Strict security & compliance** (HIPAA / GDPR — health data)
- **Mid-sized team maintainability** (5–10 engineers)

We evaluated candidate stacks across frontend, backend, database, and authentication layers (see `technology-comparison-matrix.md`).

---

## 2. Decision

We will adopt the following stack for FitFlow:

| Layer          | Technology                        |
|----------------|-----------------------------------|
| Frontend       | **Flutter**                       |
| Backend API    | **Python + FastAPI**              |
| Realtime       | **FastAPI WebSockets + Redis**    |
| AI Service     | **FastAPI + Python ML (PyTorch)** |
| Primary DB     | **PostgreSQL**                    |
| Cache / PubSub | **Redis**                         |
| Auth           | **Auth0**                         |
| Object Storage | **AWS S3**                        |
| Deployment     | **Docker + Kubernetes (EKS)**     |

---

## 3. Rationale

### Frontend — Flutter
- Single codebase for iOS, Android, and Web with a consistent UI.
- Excellent rendering performance via Skia/Impeller.
- Strong ecosystem for health/fitness widgets and charts.
- Highest weighted score (8.85) in the frontend matrix.

### Backend — FastAPI
- Native Python synergy with the AI service (no cross-language model serving).
- Async-first design suits real-time and I/O-heavy workloads.
- Auto-generated OpenAPI docs accelerate frontend integration.
- Highest weighted score (8.50) among backend candidates.

### Database — PostgreSQL + Redis
- PostgreSQL: ACID compliance, strong typing, rich indexing — ideal for structured health data.
- Redis: low-latency caching, pub/sub for realtime, session store.
- Proven HIPAA/GDPR patterns in regulated industries.

### Auth — Auth0
- Enterprise-grade OAuth2 / OIDC / MFA / SSO out of the box.
- HIPAA BAA available; GDPR-compliant data residency.
- Reduces in-house security surface area.

### AI Service — Separate FastAPI Microservice
- Isolates model inference from core API scaling.
- Allows GPU-enabled nodes only where needed.
- Independent deployment cadence for model updates.

---

## 4. Consequences

### Positive
- **Single client codebase** reduces 3× platform dev effort.
- **Unified Python stack** for API + AI reduces context-switching.
- **Compliance-ready** via PostgreSQL + Auth0 + S3 encryption.
- **Real-time capable** with Redis + WebSockets.
- **Scalable** horizontally across all layers.

### Negative / Trade-offs
- **Flutter Web** bundles are larger than a native React/Vue web app.
- Two runtimes on the backend (Python only — but ML libs can be heavy).
- **Auth0** has per-MAU pricing that grows with scale.
- Team must learn Dart (if unfamiliar) and Python async patterns.

### Mitigations
- Use `flutter build web --wasm` for smaller, faster web bundles.
- Containerize AI service separately to isolate heavy dependencies.
- Negotiate Auth0 enterprise pricing or migrate to Cognito if cost becomes prohibitive.
- Allocate 1–2 weeks onboarding for Dart + FastAPI.

---

## 5. Alternatives Considered

| Alternative Stack                            | Reason Rejected                                          |
|----------------------------------------------|----------------------------------------------------------|
| React Native + NestJS + MongoDB + Firebase   | Weaker health-data modeling; lower weighted score        |
| Kotlin Multiplatform + Go + DynamoDB + Cognito | Highest performance, but steeper learning curve and less mature ecosystem |
| Flutter + NestJS + PostgreSQL + Auth0        | Viable hybrid; rejected to keep AI + API in one language |

---

## 6. References

- `docs/technology-comparison-matrix.md` — Activity 3 weighted decision matrix
- `docs/architecture.md` — Activity 4 high-level architecture
- HIPAA Security Rule: https://www.hhs.gov/hipaa/
- GDPR Overview: https://gdpr.eu/
- Flutter Docs: https://docs.flutter.dev/
- FastAPI Docs: https://fastapi.tiangolo.com/
- Auth0 Docs: https://auth0.com/docs