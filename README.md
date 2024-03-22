## OracleOps Take Home Challenge

## Monitoring Solution for Slinky Oracle Service
This document outlines the monitoring solution designed for Slinky, Skip's premium oracle product used by high-performance app chains. The solution includes a metrics dashboard, alerts configuration, and additional features to ensure the reliability and performance of the oracle service.

## Overview
The monitoring solution is built on Prometheus for metrics collection, Grafana for visualization, and AlertManager for alert management. It offers dynamic service discovery, advanced alert correlation, anomaly detection, and automated remediation capabilities to identify and respond to issues proactively.

### Metrics

| Metric Name                             | Metric Type | Metric Description                                                                                                                                                                     |
|-----------------------------------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| oracle_aggregate_price                  | Gauge       | The price of each asset pair after running aggregation (medianization).<br/> Contains `pair` and `decimals` labels.                                                                    |
| oracle_api_response_status_per_provider | Counter     | The number of each status as reported per provider.<br/>Contains `provider`, `id`, and `status` labels.                                                                                |
| oracle_api_response_time_per_provider   | Histogram   | The response time of a API calls made as reported per provider.<br/>Contains `provider` label.                                                                                         |
| oracle_provider_last_updated_id         | Gauge       | The last updated time for each ID (currency pair).<br/>Contains `provider`, `id`, and `type` labels.                                                                                   |
| oracle_provider_price                   | Gauge       | The price that each separate provider reports (prior to aggregation).<br/>Contains `provider`, `type`, `pair`, and `decimals` labels.                                                  |
| oracle_provider_status_responses        | Counter     | The stats (success or failure) of each attempt at retrieving a price by a given provider.<br/>Contains `provider`, `status`, `code`, and `type` labels.                                |
| oracle_provider_status_responses_per_id | Counter     | The number of each status as reported per ID (currency pair).<br/>Contains `provider`, `id`, `status`, `code`, and `type` labels.                                                      |
| oracle_ticks_total                      | Counter     | The constantly incrementing number of "ticks" the oracle has successfully executed. The tick time period is configurable, but the count should continue increasing as the oracle runs. |

### Exposed services

- Prometheus: [http://localhost:9090](http://localhost:9090)
  - Accessible in Grafana through http://prometheus:9090
- Grafana: [http://localhost:3000](http://localhost:3000) (the credentials are admin / admin)
- Oracle sidecar metrics: [http://localhost:8002/metrics](http://localhost:8002/metrics)
- Oracle sidecar API: [http://localhost:8080/api](http://localhost:8080)

## Running the infrastructure
### Prerequisites
- Docker and Docker Compose
- Prometheus, Grafana, and AlertManager
- Access to the Slinky instance and related services

### Installation Steps
- Clone the Repository
```bash
git clone https://yourrepository.com/monitoring-solution.git
cd monitoring-solution
```
- Configure Services: Navigate to each service's configuration directory (prometheus, grafana, alertmanager) and review the configuration files. Update the configurations as needed to match your environment.
- Launch the Stack
```bash
docker-compose up -d
```
- Verify Installation: Ensure all services are running correctly. You can access Grafana at [http://localhost:3000](http://localhost:3000) and Prometheus at [http://localhost:9090](http://localhost:9090).

> **_Note_**
- Furthermore, you can inspect the logs of any service in the stack by running:

```bash
docker-compose logs -f <service-name>
```

## Dashboard and Visualizations
Describe the key Grafana dashboards and visualizations included in the solution, highlighting how stakeholders can use them to monitor service health, performance, and anomalies.

Explain how service discovery is configured for Prometheus to automatically detect and monitor new instances of Slinky or related services.

## Engaging with the Monitoring Solution
In engaging with the dashboard and monitoring setup, you should be aware on some major details shown as follows:

- Viewing Dashboards:
- Responding to Alerts:
- Customizing Alerts and Dashboards:

## Author
- Samuel Arogbonlo - [GitHub](https://github.com/samuelarogbonlo)

## Collaborators
- [YOUR NAME HERE] - Feel free to contribute to the codebase by resolving any open issues, refactoring, adding new features, writing test cases or any other way to make the project better and helpful to the community. Please feel free to send pull requests.


## License

The MIT License (http://www.opensource.org/licenses/mit-license.php)
