# Infrastructure

## Overview

StageScout runs on a simple, practical production stack built around one AWS-hosted Linux server. The deployment model favors low operational overhead while still using infrastructure as code, configuration management, containerization, TLS termination, and metrics collection.

## Core stack

| Layer | Choice | Responsibility |
| --- | --- | --- |
| Cloud | AWS EC2 | Compute host |
| Networking | Security Group + Elastic IP | Ingress control and stable public address |
| Provisioning | Terraform | Creates host and networking resources |
| Configuration | Ansible | Installs Docker, Nginx, Certbot, monitoring tools |
| Runtime | Docker | Packages and runs the web app |
| Edge | Nginx | Reverse proxy and TLS termination |
| TLS | Let's Encrypt | Public HTTPS certificates |
| Monitoring | Prometheus, Grafana, Node Exporter | Application and system telemetry |

## Deployment shape

```mermaid
flowchart LR
    Internet --> SG[AWS Security Group]
    SG --> EC2[Ubuntu EC2 instance]
    EC2 --> Nginx[Nginx]
    Nginx --> App[Dockerized Flask app]
    App --> Metrics[/metrics]
    Metrics --> Prom[Prometheus]
    Prom --> Graf[Grafana]
```

## Provisioning model

- Terraform allocates the EC2 instance, security group, and Elastic IP.
- SSH access is narrowed to the deployer's current public IP rather than being opened globally.
- HTTP and HTTPS remain public for the application itself.
- Monitoring ports can be limited to the operator access range.

## Host configuration

Ansible is used to turn a plain Ubuntu host into an app server with:

- Docker for running the application container
- Nginx for reverse proxying
- Certbot for certificate issuance and renewal
- Optional Prometheus, Grafana, and Node Exporter for monitoring

## Operational flow

1. Provision infrastructure.
2. Wait for SSH readiness.
3. Generate inventory for the target host.
4. Configure the machine and deploy the app container.
5. Run health checks.
6. Optionally enable TLS and monitoring.

## Security posture

- Public ingress is limited to standard web traffic.
- SSH and monitoring access can be restricted to the operator's IP.
- Secrets stay out of this public repository.
- TLS termination is handled at Nginx.
- The application itself uses secure cookie settings and server-side session storage.

## What is intentionally not published here

This public repository does not include:

- Terraform files
- Ansible playbooks or templates
- Deployment scripts
- Inventory, hostnames, or server IP data
- Docker image coordinates or registry credentials
- API keys, secrets, or private environment configuration
