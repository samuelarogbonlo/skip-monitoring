## OracleOps take home challenge

This repository contains the infrastructure required to locally set up:

- an Oracle sidecar
- a one-validator testnet chain
- Prometheus
- Grafana

For more information about the exercise itself, see the [exercise description](https://www.notion.so/skip-protocol/EXT-OracleOps-Take-Home-Assignment-665e0ec05b95452a9d173cee48dafc67?pvs=4).

### Running the infrastructure

To run the whole stack, you can run the command:

```bash
docker-compose up -d
```

Furthermore, you can inspect the logs of any service in the stack by running:

```bash
docker-compose logs -f <service-name>
```

### Exposed services

- Prometheus: [http://localhost:9090](http://localhost:9090)
  - Accessible in Grafana through http://prometheus:9090
- Grafana: [http://localhost:3000](http://localhost:3000) (the credentials are admin / admin)
- Oracle sidecar metrics: [http://localhost:8002/metrics](http://localhost:8002/metrics)
- Oracle sidecar API: [http://localhost:8080/api](http://localhost:8080)

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
