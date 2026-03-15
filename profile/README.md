# Towlion

**A self-hosted, GitHub-native micro-PaaS for deploying web applications on your own infrastructure.**

Deploy full-stack applications to a single server using nothing but GitHub repositories and Actions. No external dashboards, no managed services — complete ownership of your deployment pipeline.

---

### How It Works

```
Repository  →  GitHub Actions  →  Your Server
```

Every application is a GitHub repository. Push to `main` and it deploys automatically via SSH. Fork any app to self-host it independently on your own infrastructure.

### Platform Services

Each application runs on a single Debian server with shared infrastructure:

- **Caddy** — reverse proxy with automatic TLS
- **PostgreSQL** — relational database with per-app credential isolation
- **Redis** — caching and background job processing
- **MinIO** — S3-compatible object storage
- **Loki + Promtail** — centralized log aggregation
- **Grafana** — monitoring dashboards and alerting

### Get Started

1. Create a new app from the **[app-template](https://github.com/towlion/app-template)**
2. Build your application (FastAPI backend, optional Next.js frontend)
3. Configure server credentials in GitHub Secrets
4. Push to `main` — your app deploys automatically

### Documentation

| Repository | Description |
|---|---|
| **[platform](https://github.com/towlion/platform)** | Architecture, specification, infrastructure scripts, and validator |
| **[app-template](https://github.com/towlion/app-template)** | Template repository for bootstrapping new applications |

### Designed For

Independent developers and small teams building SaaS products, developer tools, and AI-powered applications — with full control over their production infrastructure.
