# Technology Comparison Matrix — FitFlow Redesign

**IT3060 — Human Computer Interaction | Lab Exercise 05**
**Activity 3: Consolidated Decision Matrix**

---

## 1. Frontend Framework Comparison

| Criterion (Weight)          | Flutter        | React Native   | Kotlin Multiplatform | Swift/SwiftUI |
|-----------------------------|----------------|----------------|----------------------|---------------|
| Development Speed (10%)     | 9              | 8              | 6                    | 7             |
| Code Reusability (15%)      | 10             | 9              | 8                    | 3             |
| Performance (15%)           | 9              | 7              | 9                    | 10            |
| Ecosystem Support (10%)     | 9              | 10             | 6                    | 8             |
| Learning Curve (5%)         | 7              | 8              | 5                    | 6             |
| Web Compatibility (10%)     | 8              | 7              | 4                    | 2             |
| AI/ML Integration (5%)      | 7              | 7              | 7                    | 8             |
| Real-time Features (5%)     | 8              | 8              | 8                    | 9             |
| Maintenance Cost (10%)      | 9              | 8              | 8                    | 5             |
| Security (15%)              | 9              | 8              | 9                    | 10            |
| **Weighted Total**          | **8.85**       | **7.95**       | **7.15**             | **6.55**      |

**Frontend Winner: Flutter** — best balance of reusability, performance, and multi-platform support.

---

## 2. Backend Framework Comparison

| Criterion (Weight)          | Node.js / NestJS | Python / FastAPI | Go (Gin/Fiber) |
|-----------------------------|------------------|------------------|----------------|
| Development Speed (15%)     | 9                | 9                | 6              |
| Performance (20%)           | 8                | 7                | 10             |
| Code Reusability (10%)      | 9                | 8                | 6              |
| Ecosystem Support (10%)     | 10               | 9                | 7              |
| Learning Curve (5%)         | 7                | 9                | 5              |
| AI/ML Integration (15%)     | 6                | 10               | 5              |
| Real-time Features (10%)    | 9                | 8                | 9              |
| Maintenance Cost (10%)      | 8                | 8                | 6              |
| Security (5%)               | 8                | 8                | 9              |
| **Weighted Total**          | **8.35**         | **8.50**         | **7.15**       |

**Backend Winner: Python / FastAPI** — highest AI/ML synergy and strong overall balance.  
*Note: NestJS is a strong alternative if the team prefers TypeScript end-to-end.*

---

## 3. Database Comparison

| Criterion (Weight)             | PostgreSQL | MongoDB | Firebase | DynamoDB |
|--------------------------------|------------|---------|----------|----------|
| Scalability (15%)              | 8          | 9       | 9        | 10       |
| Query Performance (20%)        | 9          | 7       | 7        | 8        |
| Health Data Handling (15%)     | 10         | 6       | 6        | 7        |
| Schema Flexibility (10%)       | 7          | 10      | 8        | 8        |
| Real-time Capabilities (10%)   | 8          | 8       | 10       | 8        |
| AI/ML Integration (10%)        | 9          | 8       | 7        | 7        |
| Cost (10%)                     | 9          | 8       | 6        | 7        |
| Security / Compliance (10%)    | 10         | 7       | 8        | 9        |
| **Weighted Total**             | **8.85**   | **7.75**| **7.60** | **8.00** |

**Database Winner: PostgreSQL** — best for structured health data, ACID compliance, and HIPAA/GDPR alignment. Pair with **Redis** for caching and real-time.

---

## 4. Authentication / Authorization Comparison

| Criterion (Weight)          | Firebase Auth | AWS Cognito | Auth0 | Supabase Auth |
|-----------------------------|---------------|-------------|-------|---------------|
| Security (25%)              | 8             | 9           | 10    | 8             |
| Compliance HIPAA/GDPR (15%) | 7             | 10          | 10    | 7             |
| Ease of Integration (15%)   | 10            | 7           | 9     | 9             |
| Real-time Capabilities (10%)| 9             | 7           | 8     | 9             |
| Cost (15%)                  | 9             | 7           | 6     | 10            |
| Maintainability (10%)       | 9             | 8           | 9     | 9             |
| AI Integration (10%)        | 6             | 8           | 9     | 7             |
| **Weighted Total**          | **8.35**      | **8.10**    | **8.85** | **8.35**  |

**Auth Winner: Auth0** — enterprise-grade security, HIPAA/GDPR compliance, smooth integration.

---

## 5. Weighted Decision Matrix — Overall Stack

| Layer        | Chosen Technology         | Score | Runner-Up          | Score |
|--------------|---------------------------|-------|--------------------|-------|
| Frontend     | **Flutter**               | 8.85  | React Native       | 7.95  |
| Backend      | **Python / FastAPI**      | 8.50  | NestJS             | 8.35  |
| Database     | **PostgreSQL + Redis**    | 8.85  | DynamoDB           | 8.00  |
| Auth         | **Auth0**                 | 8.85  | Firebase Auth      | 8.35  |
| AI Service   | **FastAPI + Python ML**   | —     | (integrated)       | —     |

**Weighted Stack Score: 8.76 / 10**

---

## 6. Final Recommendation

**Adopt a Flutter + FastAPI + PostgreSQL + Auth0 stack.**

### Justification
- **Flutter** delivers a single codebase for iOS, Android, and Web with near-native performance — critical for a fitness app with animations and real-time tracking.
- **FastAPI** provides high-performance async APIs and native synergy with Python ML libraries (TensorFlow, PyTorch, scikit-learn) for personalized workout and nutrition recommendations.
- **PostgreSQL** offers ACID compliance and structured handling of sensitive health data, essential for HIPAA/GDPR.
- **Auth0** handles complex auth flows (OAuth2, MFA, SSO) with compliance already built in.
- **Redis** adds a caching + pub/sub layer for real-time features (live workouts, social feeds).

### Trade-offs Accepted
- Two backend languages (Python + Dart/TS tooling) — mitigated by clear service boundaries.
- Flutter web bundle size is larger than a dedicated web app — acceptable for MVP.

### Hybrid Considerations
If the team has strong TypeScript expertise, **NestJS** can replace FastAPI on the main API, with FastAPI retained **only** for the AI microservice. This preserves AI/ML capability while unifying the core backend.