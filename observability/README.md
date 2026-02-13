# Observability stack

This stack adds Prometheus, Alertmanager, Grafana, Loki, Promtail, node-exporter, cAdvisor, and Blackbox Exporter.

## How to run

1. Populate `stack.env` with secrets and network-specific values (Grafana admin credentials, Alertmanager receivers, data paths).
2. From this directory: `docker compose --env-file stack.env up -d`.
3. Ensure your reverse proxy can reach Grafana on `127.0.0.1:${GRAFANA_PORT:-23000}` or attach it to the same Docker network.

## Data/volumes

Data is persisted under `/volumes/containers/observability/...` by default (configurable in `stack.env`):

- Prometheus: `/volumes/containers/observability/prometheus`
- Alertmanager: `/volumes/containers/observability/alertmanager`
- Grafana: `/volumes/containers/observability/grafana`
- Loki: `/volumes/containers/observability/loki`
- Promtail positions: `/volumes/containers/observability/promtail/positions.yaml`

## Prometheus targets

- Built-in: Prometheus, Alertmanager, node-exporter, cAdvisor, Blackbox Exporter.
- Add custom scrape targets via:
  - `prometheus/file_sd/container-targets.yml` (static host:port entries).
  - Docker labels on any container with published metrics:
    - `prometheus_scrape=true`
    - `prometheus_port=<host_port>`
    - Optional: `prometheus_path=/metrics`

## Logging

Promtail uses Docker service discovery and pushes to Loki with useful labels (compose project, service, container, host, environment). All container logs are collected by default.

## Alerts

Alert rules live in `prometheus/alerts/`. Alertmanager routes are defined in `alertmanager/alertmanager.yml` with placeholders for Gotify, email, and webhook receivers.

## Grafana provisioning

Grafana bootstraps datasources (Prometheus + Loki) and dashboards from `grafana/`. Dashboards include node-exporter overview, container overview, Prometheus health, Loki logs, and Traefik (if metrics are present).
