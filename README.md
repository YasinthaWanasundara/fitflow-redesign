# FitFlow Redesign

**IT3060 — Human Computer Interaction | Semester 2 2026**
**Lab Exercise 05 — Technology Stack Evaluation & Architecture Design**

---

## 📖 Overview

FitFlow is a fitness application that needs a **seamless iOS / Android / Web experience**, **high performance**, **real-time social features**, and **AI/ML-driven personalization** for workout plans and nutrition tracking.

This repository documents the **technology evaluation**, **weighted decision matrix**, **high-level architecture**, and **Architecture Decision Record (ADR)** produced during Lab Exercise 05.

---

## 🛠 Recommended Technology Stack

| Layer            | Technology                       | Rationale (short)                                |
|------------------|----------------------------------|--------------------------------------------------|
| Frontend         | **Flutter**                      | Single codebase; near-native perf; strong Web    |
| Backend API      | **Python + FastAPI**             | Async; AI synergy; auto OpenAPI docs             |
| Realtime         | **FastAPI WebSockets + Redis**   | Low-latency pub/sub for live features            |
| AI Microservice  | **FastAPI + PyTorch**            | Isolated inference; GPU-scalable                 |
| Primary DB       | **PostgreSQL**                   | ACID; structured health data; HIPAA-friendly     |
| Cache / Pub-Sub  | **Redis**                        | Sessions, leaderboards, realtime events          |
| Authentication   | **Auth0**                        | OAuth2/OIDC/MFA; HIPAA BAA available             |
| Object Storage   | **AWS S3**                       | Encrypted media storage                          |
| Deployment       | **Docker + Kubernetes (EKS)**    | Horizontal scale; portability                    |

> Full justification: [`docs/technology-comparison-matrix.md`](docs/technology-comparison-matrix.md)

---

## 📁 Repository Structure

```
fitflow-redesign/
├── frontend/        # Flutter client (iOS / Android / Web)
├── backend/         # FastAPI core API service
├── ai-service/      # Python AI/ML microservice
├── docs/
│   ├── technology-comparison-matrix.md   # Activity 3
│   ├── architecture.md                   # Activity 4
│   └── ADR-001-architecture.md           # Architecture Decision Record
├── README.md
└── .gitignore
```

---

## 📚 Documentation Index

| Document | Purpose |
|----------|---------|
| [Technology Comparison Matrix](docs/technology-comparison-matrix.md) | Frontend / Backend / DB / Auth comparisons + weighted decision matrix |
| [High-Level Architecture](docs/architecture.md) | System design, data flows, security, scalability |
| [ADR-001](docs/ADR-001-architecture.md) | Formal decision record for the stack selection |

---

## 🎯 Project Goals (from case study)

- ✅ Cross-platform: **iOS + Android + Web** from one codebase
- ✅ **High performance** for real-time workout tracking
- ✅ **AI-driven personalization** for workouts & nutrition
- ✅ **Real-time social features** (feed, chat, live workouts)
- ✅ **HIPAA / GDPR**-compliant health data handling
- ✅ Maintainable by a **mid-sized team (5–10 engineers)**

---

## 🚦 Project Status

**Phase:** 📄 Design & Documentation (Lab Exercise 05)

The repository currently contains the technology evaluation and architecture design. Implementation of `frontend/`, `backend/`, and `ai-service/` will begin in subsequent lab exercises.

---

## 🧪 Local Development Setup (Future)

_To be updated once the Flutter, FastAPI, and AI service projects are scaffolded._

```bash
# Frontend (Flutter)
cd frontend && flutter pub get && flutter run

# Backend (FastAPI)
cd backend && pip install -r requirements.txt && uvicorn main:app --reload

# AI Service
cd ai-service && pip install -r requirements.txt && uvicorn main:app --reload --port 8001
```

---

## 👥 Team

| Name | Role | Student ID |
|------|------|------------|
| Yasintha Wanasundara | - | IT23548978 |
| ... | ... | ... |

---

## 📜 License

Academic project — IT3060, Semester 2 2026. Not for commercial use.