# zabbix-openondemand-prometheus

This repository contains a Zabbix 7 template for monitoring Open OnDemand through the OSC `ondemand_exporter` Prometheus endpoint. The template fetches metrics locally from the Open OnDemand host using the Zabbix agent and `web.page.get`, so the exporter port does not need to be exposed to the Zabbix server or proxy.

## Included template

- [open_ondemand_by_prometheus_zabbix_7.yaml](open_ondemand_by_prometheus_zabbix_7.yaml)

## What this template monitors

The template covers the main Open OnDemand exporter metrics, including:

- Active PUNs and application counts
- Apache client and WebSocket connection totals
- PUN resource usage, including memory utilization and CPU
- Passenger dashboard metrics and request rates
- Exporter collector health, duration, timeouts, and errors
- Built-in alerting for slow collectors, connection issues, and high memory utilization

## Requirements

- Zabbix 7.x
- A Zabbix agent installed on the Open OnDemand host
- `ondemand_exporter` exposing a Prometheus endpoint, typically on `localhost:9301`
- Network access from the Zabbix agent to the local metrics endpoint

## Importing the template

1. Open the Zabbix frontend.
2. Navigate to Configuration > Templates.
3. Import [open_ondemand_by_prometheus_zabbix_7.yaml](open_ondemand_by_prometheus_zabbix_7.yaml).
4. Link the imported template to the Open OnDemand host you want to monitor.
5. Confirm the following macros are set on the host or template:
   - `{$OOD.METRICS.HOST}`: typically `localhost` or `127.0.0.1`
   - `{$OOD.METRICS.PATH}`: commonly `/metrics`
   - `{$OOD.METRICS.PORT}`: typically `9301`
   - Optional alert thresholds such as:
     - `{$OOD.PUN.MEMORY.WARN}`
     - `{$OOD.COLLECTOR.DURATION.WARN}`

## Validation

After importing the template, verify that the item `Open OnDemand: Get Prometheus metrics` returns data and that the dependent items populate correctly without requiring direct scraping from the Zabbix server.

## Notes

- This template is designed for local collection by the Zabbix agent.
- The raw HTTP response is stripped before Prometheus preprocessing is applied.
- The template includes graphs, dashboard widgets, and alerting tuned for Open OnDemand runtime and exporter health.
