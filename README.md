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
