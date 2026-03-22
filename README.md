# StageScout

> Concert discovery powered by your Spotify taste.

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-7B42BC?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Live](https://img.shields.io/badge/Live-stage--scout.fun-brightgreen)](https://stage-scout.fun)

StageScout connects to a user's Spotify account, analyzes their listening history, and surfaces live concerts near them — filtered by artist taste, location, and date range.

---

## Screenshots

**Landing**

![StageScout landing page](assets/screenshots/landing.png)

**Preferences**

![Preferences](assets/screenshots/preferences.png)

---

## How it works

1. User authenticates via **Spotify OAuth**
2. App pulls their **top artists and listening signals**
3. User selects a **location and date range**
4. Matching concerts are fetched and displayed in a focused dashboard

---

## Stack

| Area | Tools | Notes |
|---|---|---|
| Backend | Flask, Gunicorn | App factory pattern, blueprint-based routing |
| Frontend | HTML, CSS, Vanilla JS | Lightweight UI with multi-theme support |
| Data sources | Spotify, Ticketmaster, Geoapify | Music taste, event search, location autocomplete |
| State | Flask-Session, filesystem cache | Server-side sessions and cached API results |
| Infra | AWS EC2, Docker, Nginx | Single-host production deployment |
| Automation | Terraform, Ansible | Provisioning, bootstrap, and deploy flow |
| Observability | Prometheus, Grafana, Node Exporter | App and host metrics |

---

## Architecture

```mermaid
flowchart LR
    U[User Browser] --> N[Nginx]
    N --> A[StageScout Flask App]
    A --> S[Spotify Web API]
    A --> T[Ticketmaster API]
    A --> G[Geoapify API]
    A --> C[Filesystem Cache & Sessions]
    A --> M[Prometheus Metrics]
    M --> P[Prometheus]
    P --> F[Grafana]
```

---

## Deployment

```mermaid
flowchart TD
    D[deploy.sh] --> TF[Terraform]
    TF --> AWS[AWS EC2 + Security Group + Elastic IP]
    D --> AN[Ansible]
    AN --> HOST[Ubuntu host]
    HOST --> NG[Nginx]
    HOST --> DOCKER[Docker container]
    DOCKER --> APP[Flask + Gunicorn]
```

Full deployment docs in [`docs/`](docs/).

---

## License

MIT — see [LICENSE](LICENSE).
