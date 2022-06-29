# Documentation
## Prerequisites

- Git
- Docker & Docker Compose 1.29.1
- Ports `9000` and `9009` available on your host machine

## Download configuration

- Clone the repository  using the Git command line:

```
git clone https://github.com/grafana/mimir.git
cd mimir
```

- Use `cd` to navigate to the docker compose file:

```
cd cd docs/sources/tutorials/play-with-grafana-mimir/
```

## Start Grafana Mimir & dependencies

```
docker-compose up
```
The following command starts:

- Grafana Mimir 
- Minio
- Prometheus
- Grafana
- Load balancer






