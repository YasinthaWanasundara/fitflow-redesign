# High-Level Architecture — FitFlow Redesign

**IT3060 — Human Computer Interaction | Lab Exercise 05**
**Activity 4: System Architecture**

---

## 1. Architecture Overview

FitFlow uses a **microservice-oriented architecture** with a cross-platform Flutter client, a Python/FastAPI core API, a dedicated AI microservice, PostgreSQL as the primary datastore, and Redis for caching and real-time pub/sub.

```mermaid
flowchart TB
    subgraph Client["Client Layer"]
        F1[Flutter iOS]
        F2[Flutter Android]
        F3[Flutter Web]
    end

    subgraph Edge["Edge / Gateway"]
        CDN[CDN - Static Assets]
        GW[API Gateway / Load Balancer]
    end

    subgraph Backend["Backend Services"]
        API[FastAPI Core API]
        RT[WebSocket / Realtime Service]
        NOTIF[Notification Service]
    end

    subgraph AI["AI Microservice"]
        AIS[FastAPI AI Service]
        ML[ML Models<br/>Workout + Nutrition]
    end

    subgraph Data["Data Layer"]
        PG[(PostgreSQL<br/>Primary DB)]
        RD[(Redis<br/>Cache + Pub/Sub)]
        S3[(Object Storage<br/>Media / Videos)]
    end

    subgraph External["External Services"]
        AUTH[Auth0]
        PAY[Payment Gateway]
        PUSH[Push Notifications]
    end

    F1 & F2 & F3 --> CDN
    F1 & F2 & F3 --> GW
    GW --> API
    GW --> RT
    API --> PG
    API --> RD
    API --> S3
    API --> AIS
    API --> AUTH
    API --> PAY
    RT --> RD
    AIS --> ML
    AIS --> PG
    NOTIF --> PUSH
    API --> NOTIF
```

---

## 2. Key Components

| Component              | Technology              | Responsibility                                      |
|------------------------|-------------------------|-----------------------------------------------------|
| Client                 | Flutter                 | iOS, Android, Web UI                                |
| API Gateway            | Nginx / Cloud LB        | Routing, rate limiting, TLS termination             |
| Core API               | Python / FastAPI        | Business logic, CRUD, orchestration                 |
| Realtime Service       | FastAPI + WebSockets    | Live workouts, chat, social feed                    |
| AI Microservice        | FastAPI + Python ML     | Personalized plans, nutrition analysis              |
| Primary Database       | PostgreSQL              | Users, workouts, nutrition logs, social data        |
| Cache / Pub-Sub        | Redis                   | Sessions, leaderboards, real-time events            |
| Object Storage         | S3 / GCS                | Videos, images, user avatars                        |
| Authentication         | Auth0                   | OAuth2, MFA, SSO, JWT issuance                      |
| Notifications          | FCM / APNs              | Push notifications                                  |

---

## 3. Critical Data Flows

### 3.1 Personalized Workout Plan

```mermaid
sequenceDiagram
    participant U as User (Flutter)
    participant A as Core API
    participant AI as AI Service
    participant DB as PostgreSQL
    participant C as Redis Cache

    U->>A: POST /workout/generate {goals, history}
    A->>C: Check cached plan
    alt Cache miss
        A->>AI: POST /predict {user features}
        AI->>DB: Fetch user history
        AI-->>A: Recommended plan
        A->>DB: Save plan
        A->>C: Cache plan (TTL 24h)
    end
    A-->>U: 200 OK {plan}
```

### 3.2 Social Sharing

```mermaid
sequenceDiagram
    participant U as User
    participant A as Core API
    participant DB as PostgreSQL
    participant R as Redis Pub/Sub
    participant RT as Realtime Service
    participant N as Notification Service

    U->>A: POST /posts {workout, image}
    A->>DB: Insert post
    A->>R: Publish "new_post" event
    R-->>RT: Broadcast to followers
    RT-->>U: Push update to feeds
    A->>N: Trigger push notification
    N-->>U: FCM/APNs push
```

### 3.3 Nutrition Tracking

```mermaid
sequenceDiagram
    participant U as User
    participant A as Core API
    participant AI as AI Service
    participant DB as PostgreSQL

    U->>A: POST /nutrition/log {meal, macros}
    A->>DB: Save log
    A->>AI: POST /analyze {log, daily totals}
    AI-->>A: Insights + suggestions
    A-->>U: 200 OK {insights}
```

---

## 4. Security Considerations

- **Transport:** TLS 1.3 everywhere (HTTPS + WSS).
- **Auth:** Auth0-issued JWTs, short-lived access tokens, refresh token rotation.
- **Authorization:** Role-based access control (RBAC) — user, coach, admin.
- **Data at Rest:** PostgreSQL encryption at rest (AES-256); S3 SSE.
- **Compliance:** HIPAA-aligned audit logs; GDPR data export + right-to-be-forgotten endpoints.
- **Secrets:** Managed via AWS Secrets Manager / HashiCorp Vault.
- **Rate Limiting:** Redis-backed per-user + per-IP throttling at the gateway.

---

## 5. Scalability Considerations

- **Horizontal scaling:** Stateless FastAPI pods behind a load balancer (Kubernetes / ECS).
- **Database:** Read replicas for analytics; partitioning of large tables (nutrition logs).
- **Cache:** Redis cluster mode for high throughput.
- **AI service:** Separate autoscaling group — GPU-enabled nodes if needed.
- **CDN:** Static assets and media served via CDN to reduce origin load.
- **Async work:** Background jobs (email, notifications, ML training) via Celery or Arq.

---

## 6. Integration Considerations

- **CI/CD:** GitHub Actions → Docker → container registry → deployment.
- **Observability:** Prometheus + Grafana (metrics), Loki / ELK (logs), OpenTelemetry (traces).
- **Versioning:** API versioned via `/api/v1/...` prefix.
- **Feature flags:** LaunchDarkly or Unleash for progressive rollout.
- **Testing:** Unit + integration (pytest), E2E (Flutter integration_test), load (k6).

---

## 7. Deployment Topology (High-Level)

```
[ Flutter App ] ──► [ CDN ] ──► [ API Gateway ]
                                     │
              ┌──────────────────────┼──────────────────────┐
              ▼                      ▼                      ▼
        [ FastAPI Core ]      [ Realtime WS ]        [ AI Service ]
              │                      │                      │
              └──────────┬───────────┴──────────┬───────────┘
                         ▼                      ▼
                    [ PostgreSQL ]         [ Redis ]
                         │
                    [ S3 / GCS ]
```