---
title: "Agents Gateway — open-source FastAPI gateway for AI agents"
description: "Production FastAPI gateway for AI agents on top of Agno. Supervisor/worker execution, agent teams, prompt versioning, OAuth toolkits. MIT. One-click deploy."
---

# Agents Gateway

<p class="lead">
A production-ready, MIT-licensed FastAPI service for serving, orchestrating,
and managing AI agents — built on <a href="https://github.com/agno-ai/agno">Agno</a>.
Deploy in one click; scale to your own Kubernetes when you outgrow it.
</p>

<div class="cta-row">
  <a class="btn" href="#quickstart">Quickstart</a>
  <a class="btn secondary" href="/docs/">Docs</a>
  <a class="btn secondary" href="https://github.com/liberzon/agents-gateway" rel="noopener">GitHub</a>
</div>

<div class="deploy-buttons">
  <a href="https://railway.app/template?template=https://github.com/liberzon/agents-gateway-deploy" rel="noopener">
    <img alt="Deploy on Railway" src="https://railway.app/button.svg">
  </a>
  <a href="https://render.com/deploy?repo=https://github.com/liberzon/agents-gateway-deploy" rel="noopener">
    <img alt="Deploy to Render" src="https://render.com/images/deploy-to-render-button.svg">
  </a>
  <a href="https://app.koyeb.com/deploy?type=git&repository=github.com/liberzon/agents-gateway-deploy" rel="noopener">
    <img alt="Deploy to Koyeb" src="https://img.shields.io/badge/Deploy%20to-Koyeb-121212?style=for-the-badge&logo=koyeb&logoColor=white">
  </a>
</div>

## What you get

<div class="features">
  <div class="feature">
    <h3>Agent management</h3>
    <p>Create, configure, version, and serve agents via a REST API. Per-agent prompts, models, toolkits, knowledge.</p>
  </div>
  <div class="feature">
    <h3>Teams</h3>
    <p>Compose agents into teams for coordinated multi-agent workflows. Coordinate or supervisor modes.</p>
  </div>
  <div class="feature">
    <h3>Supervisor platform</h3>
    <p>Job queue, approval flows, containerized workers (Docker &amp; K8s). Production-ready execution.</p>
  </div>
  <div class="feature">
    <h3>Toolkits</h3>
    <p>Pre-built OAuth-managed integrations for Calendar, Email, Contacts, and Drive — Google &amp; Microsoft.</p>
  </div>
  <div class="feature">
    <h3>Prompts service</h3>
    <p>Versioned prompt templates with pluggable storage (PostgreSQL, LangSmith). Backend-aware.</p>
  </div>
  <div class="feature">
    <h3>Knowledge base</h3>
    <p>Per-tenant document indexing on Qdrant. Tag and retrieve at agent or team scope.</p>
  </div>
  <div class="feature">
    <h3>Observability</h3>
    <p>Pluggable OpenTelemetry tracing and logging. Console, OTLP, Sentry, Logtail out of the box.</p>
  </div>
  <div class="feature">
    <h3>One-click deploy</h3>
    <p>Render, Railway, Koyeb buttons. Pre-built image on GHCR (linux/amd64 + arm64). K8s manifests included.</p>
  </div>
</div>

## Quickstart {#quickstart}

```sh
git clone https://github.com/liberzon/agents-gateway
cd agents-gateway

# Start PostgreSQL + Qdrant (seeds demo agents automatically)
docker compose up -d

# Set up Python environment
./scripts/dev_setup.sh && source .venv/bin/activate

# Start the API
./scripts/start_server.sh
```

Then chat with a demo agent:

```sh
export GOOGLE_API_KEY="your-gemini-key"

curl -X POST http://localhost:8000/v2/agents/demo-assistant/chat \
  -H 'Content-Type: application/json' \
  -d '{"message":"What can you help me with?","user_id":"u1","session_id":"s1","timezone":"UTC","locale":"en","stream":false}'
```

Interactive API docs at `http://localhost:8000/docs`.

## Deploy

Three one-click providers backed by a prebuilt multi-arch image:

| Provider | Config |
|---|---|
| [Render](https://render.com/deploy?repo=https://github.com/liberzon/agents-gateway-deploy) | `render.yaml` — image-based |
| [Railway](https://railway.app/template?template=https://github.com/liberzon/agents-gateway-deploy) | `railway.toml` — wrapper Dockerfile |
| [Koyeb](https://app.koyeb.com/deploy?type=git&repository=github.com/liberzon/agents-gateway-deploy) | `koyeb.yaml` — image-based |

Self-hosters can run the Kustomize-based [K8s manifests](https://github.com/liberzon/agents-gateway-deploy/tree/main/k8s). Cloud-specific manifests for AWS, Azure, and GCP live in [`agents-gateway/deploy/`](https://github.com/liberzon/agents-gateway/tree/main/deploy).

Full per-platform playbook: [Deploy guide](/deploy/).

## Project links

- **Source code** — [github.com/liberzon/agents-gateway](https://github.com/liberzon/agents-gateway)
- **Deploy artifacts** — [github.com/liberzon/agents-gateway-deploy](https://github.com/liberzon/agents-gateway-deploy)
- **Container image** — `ghcr.io/liberzon/agents-gateway:latest`
- **License** — [MIT](https://github.com/liberzon/agents-gateway/blob/main/LICENSE)
- **Issues** — [Report a bug](https://github.com/liberzon/agents-gateway/issues)

## FAQ

### Is it production-ready?
Yes, with caveats. The gateway, supervisor, queue, and toolkits are covered by an integration test suite that runs against real Postgres/Qdrant on every push. Pick your own observability and harden the auth before shipping to users.

### How does it relate to Agno?
Agents Gateway is a service layer **on top of** [Agno](https://github.com/agno-ai/agno). Agno provides the agent primitives (LLM calls, tools, sessions); the gateway provides the HTTP API, multi-tenancy, persistence, supervisor orchestration, and operational surface.

### What models does it support?
Anything Agno supports — OpenAI, Anthropic, Gemini, plus other providers. Configure per agent via the `model` field.

### Does it need a vector DB?
Qdrant is bundled via docker-compose for the knowledge base. If you don't use knowledge entries, you can run without it. The chat path doesn't depend on Qdrant.

### Can I run it without docker?
Yes — you need Postgres 14+ and (optionally) Qdrant reachable over the network. See [Deploy](/deploy/).
