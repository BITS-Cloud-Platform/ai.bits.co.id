<div align="center">
  <h1>9Router</h1>
  <p><strong>AI Gateway & Routing Proxy</strong></p>
  <p>
    <a href="https://ai.bits.co.id" target="_blank">ai.bits.co.id</a> ·
    <a href="https://bits.co.id" target="_blank">Banten IT Solutions</a>
  </p>
  <br>
</div>

---

## 📋 Overview

**9Router** is a lightweight, Docker-based AI routing and proxying gateway. It serves as the entry point for AI services, providing authentication, request routing, observability, and secure access management — all behind a single endpoint.

It is currently deployed and running live at **[ai.bits.co.id](https://ai.bits.co.id)**, powering AI workloads for [Banten IT Solutions](https://bits.co.id).

---

## 🏗️ Architecture

```
                    ┌─────────────┐
                    │  9Router    │  (Port 20128)
                    │  AI Gateway │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │ Headroom │ │ SearXNG  │ │   ...    │
        │ Sidecar  │ │  Search  │ │ Services │
        └──────────┘ └──────────┘ └──────────┘
```

| Service       | Role                                   |
|---------------|----------------------------------------|
| **9Router**   | Main AI gateway & request router       |
| **Headroom**  | Observability & monitoring sidecar     |
| **SearXNG**   | Private meta-search engine             |

---

## 🚀 Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/) (v24+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2.20+)

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/your-org/9router.git
cd 9router

# 2. Configure environment variables
cp .env.example .env
# ⚠️  Edit .env and set your own secrets (JWT_SECRET, API_KEY_SECRET, etc.)

# 3. Start the services
docker compose up -d

# 4. Verify the deployment
curl http://localhost:20128/api/health
```

---

## ⚙️ Configuration

All configuration is managed through environment variables (`.env` file).

| Variable                  | Default                | Description                          |
|---------------------------|------------------------|--------------------------------------|
| `PORT`                    | `20128`                | Application port                     |
| `NODE_ENV`                | `production`           | Environment mode                     |
| `JWT_SECRET`              | _(required)_           | Secret key for JWT signing           |
| `API_KEY_SECRET`          | _(required)_           | Secret key for API key hashing       |
| `MACHINE_ID_SALT`         | _(required)_           | Salt for machine ID generation       |
| `INITIAL_PASSWORD`        | _(required)_           | Default admin password               |
| `BASE_URL`                | —                      | Public-facing base URL               |
| `CLOUD_URL`               | —                      | Cloud service URL                    |
| `ENABLE_REQUEST_LOGS`     | `false`                | Toggle request logging               |
| `OBSERVABILITY_ENABLED`   | `true`                 | Enable metrics & monitoring          |
| `AUTH_COOKIE_SECURE`      | `true`                 | Set Secure flag on cookies           |
| `REQUIRE_API_KEY`         | `true`                 | Require API key for all requests     |
| `HEADROOM_URL`            | `http://headroom:8787` | Headroom sidecar URL                 |
| `SEARXNG_URL`             | —                      | SearXNG search engine URL            |

---

## 🐳 Docker Deployment

### Production

```bash
docker compose up -d
```

### With Custom Resources

```yaml
# docker-compose.override.yml (not tracked by git)
services:
  9router:
    deploy:
      resources:
        limits:
          memory: 1G
          cpus: "2.0"
```

### Health Check

```bash
curl https://ai.bits.co.id/api/health
```

Expected response:
```json
{"status":"ok"}
```

---

## 🔐 Security

> **⚠️ IMPORTANT**: Before pushing to any public repository, ensure you have:
> 1. Created a `.env` file with **your own unique secrets**
> 2. Added `.env` to `.gitignore` (it is by default)
> 3. Never committed the actual `.env` file to version control

### Best Practices

- Rotate `JWT_SECRET`, `API_KEY_SECRET`, and `MACHINE_ID_SALT` regularly
- Use strong, randomly generated values for all secrets
- Enable `AUTH_COOKIE_SECURE=true` in production (requires HTTPS)
- Restrict network access to the service port

---

## 📊 Observability

When `OBSERVABILITY_ENABLED=true` (default), the Headroom sidecar collects and exposes metrics:

- Request counts & latency
- Error rates
- Active connections
- System resource usage

---

## 🧑‍💻 Development

```bash
# View logs
docker compose logs -f 9router

# Restart a service
docker compose restart 9router

# Stop all services
docker compose down

# Reset data volume
docker compose down -v
```

---

## 🧪 Testing

```bash
# Health endpoint
curl http://localhost:20128/api/health

# With API key
curl -H "X-API-Key: your-api-key" http://localhost:20128/api/health
```

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feat/your-feature`)
3. Commit your changes (`git commit -m 'feat: add feature'`)
4. Push to the branch (`git push origin feat/your-feature`)
5. Open a Pull Request

---

## 📄 License

This project is developed and maintained by **Banten IT Solutions**.

---

<div align="center">
  <p>
    <strong>9Router</strong> ·
    <a href="https://ai.bits.co.id">ai.bits.co.id</a> ·
    <a href="https://bits.co.id">bits.co.id</a>
  </p>
  <p>
    Made with ❤️ by <a href="https://bits.co.id"><strong>Banten IT Solutions</strong></a>
  </p>
  <br>
  <p>
    <img src="https://img.shields.io/badge/status-live-success" alt="Status">
    <img src="https://img.shields.io/badge/version-1.0.0-blue" alt="Version">
    <img src="https://img.shields.io/badge/docker-ready-2496ED?logo=docker" alt="Docker">
  </p>
</div>
