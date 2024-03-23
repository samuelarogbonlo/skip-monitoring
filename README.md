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
- Create a personal server on any cloud platform of your choice or use your local machine.
- Access the created server or use your local machine terminal to clone the Repository by running the following command:
```bash
git clone https://yourrepository.com/monitoring-solution.git
cd monitoring-solution
```
- **Configure Services:** Navigate to each service's configuration directory (prometheus, grafana, alertmanager) and review the configuration files. Update the configurations as needed to match your environment.

- Navigate to `alertmanager.yml` file in the alertmanager directory then update the `channel` with your slack channel name and `api_url` with your slack webhook for that channel. Check more information on setting up a webhook URL [here](https://api.slack.com/messaging/webhooks)

- **Launch the Stack:** This will start all the services and monitoring setup as well.

```bash
docker-compose up -d
```

- Verify Installation: Ensure all services are running correctly. You can access Grafana at [http://localhost:3000](http://localhost:3000) and Prometheus at [http://localhost:9090](http://localhost:9090).

- For access, you can use the default username and password as `admin`, but you have a choice of changing the password after the first password usage.

> **_Note_**
> - Furthermore, you can inspect the logs of any service in the stack by running:

```bash
docker-compose logs -f <service-name>
```

## Dashboard and Visualizations
On the dashboard, there are three major metric sections and they are listed thus:
- **_Provider API Metrics:_** This includes the "total" number of provider responses by status per hour and by ID per hour. To interact with both panels, you can make changes to **`provider`**, **`Provider API Status`**, and **`id`** variables. In summary, the two panels are listed below:
     - **_Provider Responses By Status Per Hour:_** This provides introspection into how often providers are successfully updating their data.
     - **_Provider Responses By ID Per Hour:_** This provides introspection into how often each price feed is being updated successfully.

- **_Base Provider Metrics:_** This row has two major panels as well and to modify and compare data, you need to make changes to the **`provider`**, **`Base Provider Status`** and **`id`** variables. The panels are listed thus:
     - **_Average Number of Responses Per Provider And Status Per Hour_**
     - **_Average Number of Responses Per ID Per Hour_**

- **_Prices & Charts:_** This part of the dashboard has six panels and they have different functions for the user. To interact with these panels, you can make changes to the **`pair`**, **`type`**, **`provider`**, They include the following:
     - **_Oracle Aggregate Price Chart:_** This shows the oracle aggregate price chart over time
     - **_Oracle Provider Price Chart:_** This displays the oracle provider price chart over a certain timeframe.
     - **_Oracle Aggregate Price:_** This displays the Oracle aggregate price over time.
     - **_Oracle Provider Price:_** This displays the oracle's provider price per time.
     - **_Oracle Provider Last Updated Time For Each Currency Pair In Seconds:_** This is the time taken for oracle provider API to update currency pair data.
     - **_Rate of Oracle Ticks Per Hour:_** This displays the rate of total ticks per hour in the setup runtime.

- **_Miscellaneous:_** This row has two panels. To modify the panels, you'll make changes to the variables; **`id`**, **`provider`**, **`type`**. They include:
     - **_Oracle Provider Last Updated Time For Each Currency Pair in Seconds:_** Time taken for the oracle provider API to update currency pair data.
     - **_Rate of Oracle Ticks:_** Displays rate of oracle ticks per hour.

Generally, stakeholders would find the dashboard very helpful as it highlights different price variances and peculiarities per time. We went forward to set up "Rate of Oracle Ticks" to monitor the spikes in the infrastructure so we can rightly be alerted when things get out of hand. Furthermore, alerting is very crucial to having visibility status of the entire stack and we have built alerting rules that could still be expanded as the stack expands - this also promotes solid service discovery. The rules are listed thus:

- Oracle Service Anomalies
- High Error Rates Critical
- Significant Response Time Increases
- Significant Response Time Increases(Critical)
- Data Freshness Issues
- Price Data Anomalies
- Service Unavailability
- Spike In Query Volume

> **_Note_**
> - Be aware that all the descriptions for the alerts are added in the rules file [here](https://github.com/samuelarogbonlo/oracleops-take-home/blob/main/contrib/prometheus/rules.yml). And any other necessary alerts can be added as we move forward.
> - In engaging with the dashboard and monitoring setup, you should be aware that all you have to do is change the different variables to fit your preference of display and everything works as an out-of-the-box solution.
> - Other alert-receiving platforms like Discord and email can be set as well. 

## Author
- Samuel Arogbonlo - [GitHub](https://github.com/samuelarogbonlo)
