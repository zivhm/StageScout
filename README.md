# StageScout

StageScout is a concert discovery app that turns a user's Spotify taste into live event recommendations.

This public repository is a product and architecture showcase for the live app at [stage-scout.fun](https://stage-scout.fun).

![StageScout landing page](assets/screenshots/landing.png)

## What the app does

1. Connects to Spotify with OAuth.
2. Pulls a user's top artists and listening signals.
3. Lets the user pick a location and date range.
4. Finds matching concerts and presents them in a focused dashboard.

## Stack

| Area | Tools | Notes |
| --- | --- | --- |
| Backend | Flask, Gunicorn | App factory pattern with blueprint-based routing |
| Frontend | HTML, CSS, Vanilla JavaScript | Lightweight UI with multi-theme support |
| Data sources | Spotify, Ticketmaster, Geoapify | Identity, music taste, event search, location autocomplete |
| State | Flask-Session, filesystem cache | Server-side sessions and cached API results |
| Infra | AWS EC2, Docker, Nginx | Single-host production deployment |
| Automation | Terraform, Ansible | Provisioning, server bootstrap, deploy flow |
| Observability | Prometheus, Grafana, Node Exporter | App and host metrics |

## Product walkthrough

| Landing | Preferences |
| --- | --- |
| ![Landing](assets/screenshots/landing.png) | ![Preferences](assets/screenshots/preferences.png) |

## Architecture

```mermaid
flowchart LR
    U[User Browser] --> N[Nginx]
    N --> A[StageScout Flask App]
    A --> S[Spotify Web API]
    A --> T[Ticketmaster API]
    A --> G[Geoapify API]
    A --> C[Filesystem Cache and Sessions]
    A --> M[Prometheus Metrics]
    M --> P[Prometheus]
    P --> F[Grafana]
```

## Deployment model

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

## License

MIT. See [LICENSE](LICENSE).
