# Towlion

**Self-hosted micro-PaaS for deploying web applications directly from GitHub.**

No dashboards. No complex infrastructure. Just push code and deploy.

---

### How it works

```
your repo  -->  GitHub Actions  -->  your server
```

Every application is a GitHub repository. Push to `main` and it deploys automatically to your own server via SSH. Fork any app to self-host it on your own infrastructure.

### What's included

Each application runs on a single Debian server with shared infrastructure:

- **Caddy** for reverse proxy and automatic TLS
- **PostgreSQL** for databases
- **Redis** for caching and background jobs
- **MinIO** for S3-compatible object storage

### Get started

1. Create a new app from the **[app-template](https://github.com/towlion/app-template)** (click "Use this template")
2. Write your application code (FastAPI backend, optional Next.js frontend)
3. Configure your server secrets in GitHub
4. Push to `main` — your app is live

### Repositories

| Repo | Purpose |
|------|---------|
| **[platform](https://github.com/towlion/platform)** | Architecture docs, spec, validator, governance |
| **[app-template](https://github.com/towlion/app-template)** | Template repo for bootstrapping new apps |

### Built for

Small SaaS apps, developer tools, AI-generated applications, and personal product ecosystems. Optimized for indie developers who want production infrastructure without the complexity.
