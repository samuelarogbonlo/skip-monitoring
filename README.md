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
- Create your personal server on any cloud platform of choice or use your local machine but be aware that the image files are large.
- Access the created server or use your local machine terminal to clone the Repository by running the following command:
```bash
git clone https://yourrepository.com/monitoring-solution.git
cd monitoring-solution
```
- Configure Services: Navigate to each service's configuration directory (prometheus, grafana, alertmanager) and review the configuration files. Update the configurations as needed to match your environment.
- Launch the Stack: This will start all the services and monitoring setup as well.
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
On the dashboard, there are three major metric sections and they are listed thus:
- Provider API Metrics: This includes the "total" number of provider responses by status per hour and by ID per hour. For the both panels, there are just two panel status which is `success` and `unknown_err` regardless of if the dashboard is set to `success` or `failure`. In summary, while the `status by hour` provides introspection into how often providers are successfully updating their data, the `ID per hour` provides introspection into how often each price feed is being updated successfully.

- Base Provider Metrics: This has two major panels as well and while one gives the "average" number of responses per provider and status per hour, the latter the average number of responses per ID per hour.

- Prices & Charts: This part of the dashboard has six panels and they hav different functions to the user. They include oracle aggregate price chart, oracle provicer price chart, Oracle aggregate price, Oracle provider price, oracle provider last updated time for each currency pair in seconds and finally the rate of oracle ticks per hour.

Generally, stakeholders would find the dashboard very helpful as it does highlight different price variances and peculiarities per time. We even went forward to even setup rate of Oracle ticks to monitor the spikes in the infrastructure so we can rightly be alerted when things get our of hand. furthermore, alerting is very crucial to having visibility status of the entire stack and we have build alerting rules that could still be expanded as the stack expands - this also promites solid service discovery. The rules are listed thus:

- Oracle Service Anomalies
- High Error Rates Critical
- Significant Response Time Increases
- Significant Response Time IncreasesCritical
- Data Freshness Issues
- Price Data Anomalies
- Service Unavailability
- Spike In Query Volume

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
