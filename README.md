# PISRI_demo

[EN]

# PISRI — Public Summary (Sanitised)

This file is a public, organised and secure version of the PISRI (Integrated Security and Incident Response Platform) project.

## Quick overview

PISRI integrates perimeter protection (WAF), rule-based detection, response automation (SOAR), and observability (logs + metrics). It is designed to defend web applications and APIs, automate reactive actions, and provide operational visibility.

---

## Main components

- Gateway — Nginx + ModSecurity (OWASP CRS): reverse proxy with traffic inspection and rate limiting.
- Central API — FastAPI: event ingestion, alert/incident management, administrative endpoints (e.g., reload keys, approvals).
- Detector — rule engine that consumes logs (Loki) and triggers alerts when rules are satisfied.
- SOAR — playbook executor that applies automatic or semi-automatic actions (e.g., block IP, notify, create ticket).
- Threat-Intel Updater — updates blocklists/indicators consumed by the gateway.
- Observability — Loki (logs), Prometheus (metrics), Grafana (dashboards), Alertmanager (alert routing), Blackbox exporter (probes).
- Support infrastructure — PostgreSQL (persistence), Redis (state/queues), Vector/Promtail (forwarders), volumes for blocklists (`ipdeny`).

---

## Features (detailed)

### Perimeter protection
- WAF configured with OWASP CRS (configurable detection/prevention mode).
- Rate limiting and IP policies in Nginx.
- Support for integrable CAPTCHA challenges (hCaptcha / Cloudflare Turnstile) via challenge proxy.

### Ingestion & Detection
- Event ingestion endpoint (`/events`) with HMAC support (signature verification) and quotas per key.
- Configurable rule engine via YAML with thresholds, cooldowns, deduplication, and allowlists to reduce false positives.
- Example rules for: SQLi bursts, authentication failures, modsecurity audit bursts, high 5xx rate, suspicious path traversal.

### SOAR & Orchestration
- Sample playbooks that automate responses (temporary IP block, notify via Discord/Jira, create ticket).
- Pending/approval mechanism: sensitive actions may require human approval (pending items in Redis with TTL).
- Supported actions (examples): `block_ip`, `notify_discord`, `jira`, `webhook`, `email`, `cloudflare_mode`.

### Observability & Alerting
- Pre-provisioned dashboards in Grafana (e.g., blackbox, detector metrics).
- Alertmanager with routing rules; optional relay to Discord.
- Blackbox exporter for availability and latency checks.

  ### Management & Operations
- Endpoints for creating/listing/incidents, commenting on incidents, and ingesting vulnerability reports (e.g., Trivy JSON).
- Smoke test and health check scripts included to facilitate local validation.

---

## Technologies

Below is a summary of the technologies and components used in the project (or planned for the stack):

- Gateway / WAF: Nginx, ModSecurity, OWASP CRS
- API / Backend: Python, FastAPI, SQLAlchemy
- Database: PostgreSQL
- Cache / State: Redis
- Logging / Observability: Grafana, Loki, Prometheus, Alertmanager, Promtail
- Forwarding / Syslog: Vector
- Containerisation / Local orchestration: Docker, Docker Compose
- Detector / SOAR: Python-based engines and YAML playbooks (integration via Redis/HTTP)
- Uploads / Proxies: CAPTCHA support (hCaptcha / Cloudflare Turnstile) and integration with external services (Discord webhooks, Jira API, Cloudflare API)
- Exporters/Probes: Blackbox exporter (Prometheus)
- Utilities: httpx, pydantic, yaml, psycopg2/async drivers (depending on the service).

  <br/>   <br/>   <br/>   <br/> 

[PT]

# PISRI — Resumo Público (Sanitizado)

Este ficheiro é uma versão pública, organizada e segura do projecto PISRI (Plataforma Integrada de Segurança e Resposta a Incidentes).

## Visão rápida

PISRI integra proteção perimetral (WAF), deteção baseada em regras, automação de resposta (SOAR) e observabilidade (logs + métricas). Destina-se a defender aplicações web e APIs, automatizar ações reativas e fornecer visibilidade operacional.

---

## Componentes principais

- Gateway — Nginx + ModSecurity (OWASP CRS): proxy reverso com inspeção de tráfego e rate limiting.
- API central — FastAPI: ingestão de eventos, gestão de alertas/incident, endpoints administrativos (ex.: reload keys, approvals).
- Detector — regra-engine que consome logs (Loki) e dispara alertas quando regras são satisfeitas.
- SOAR — executor de playbooks que aplica ações automáticas ou semi-automáticas (ex.: block IP, notificar, criar ticket).
- Threat-Intel Updater — actualiza blocklists/indicadores consumidos pelo gateway.
- Observability — Loki (logs), Prometheus (métricas), Grafana (dashboards), Alertmanager (roteamento de alertas), Blackbox exporter (probes).
- Infra de suporte — PostgreSQL (persistência), Redis (estado/queues), Vector/Promtail (forwarders), volumes para blocklists (`ipdeny`).

---

## Funcionalidades (detalhado)

### Proteção perimetral
- WAF configurado com OWASP CRS (modo detection/prevention configurável).
- Rate limiting e políticas por IP no Nginx.
- Suporte para desafios CAPTCHA integráveis (hCaptcha / Cloudflare Turnstile) via proxy de challenge.

### Ingestão & Deteção
- Endpoint de ingestão de eventos (`/events`) com suporte a HMAC (verificação de assinatura) e quotas por chave.
- Rule-engine configurável via YAML com thresholds, cooldowns, deduplication e allowlists para reduzir falsos positivos.
- Regras exemplo para: bursts de SQLi, falhas de autenticação, modsecurity audit bursts, alta taxa de 5xx, path traversal suspeito.

### SOAR & Orquestração
- Playbooks exemplares que automatizam respostas (block temporário de IP, notificar via Discord/Jira, criar ticket).
- Mecanismo de pendências/approvals: ações sensíveis podem requerer aprovação humana (pendências em Redis com TTL).
- Ações soportadas (exemplos): `block_ip`, `notify_discord`, `jira`, `webhook`, `email`, `cloudflare_mode`.

### Observability & Alerting
- Dashboards pre-provisionados em Grafana (e.g., blackbox, detector metrics).
- Alertmanager com regras de roteamento; relay opcional para Discord.
- Blackbox exporter para verificações de disponibilidade e latência.

### Gestão & Operações
- Endpoints para criar/listar/incidentes, comentar incidentes, e ingestão de relatórios de vulnerabilidades (ex.: Trivy JSON).
- Scripts de smoke test e healthchecks incluídos para facilitar validação local.

---

## Tecnologias

Abaixo está um resumo das tecnologias e componentes usados no projecto (ou previstos pela stack):

- Gateway / WAF: Nginx, ModSecurity, OWASP CRS
- API / Backend: Python, FastAPI, SQLAlchemy
- Base de dados: PostgreSQL
- Cache / State: Redis
- Logging / Observability: Grafana, Loki, Prometheus, Alertmanager, Promtail
- Forwarding / Syslog: Vector
- Containerização / Orquestração local: Docker, Docker Compose
- Detector / SOAR: motores baseados em Python e playbooks YAML (integração via Redis/HTTP)
- Uploads / Proxies: suporte para CAPTCHA (hCaptcha / Cloudflare Turnstile) e integração com serviços externos (Discord webhooks, Jira API, Cloudflare API)
- Exporters / Probes: Blackbox exporter (Prometheus)
- Utilitários: httpx, pydantic, yaml, psycopg2 / async drivers (dependendo do serviço)


