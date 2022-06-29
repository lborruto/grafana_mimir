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

![diagram](https://grafana.com/tutorials/play-with-grafana-mimir/tutorial-architecture.png)

The following ports will be exposed on the host:

- Grafana on `http://localhost:9000`
- Grafana Mimir on `http://localhost:9009`

## Configure a recording rule

1. Open Grafana Alerting.
2. Click “New alert rule”, which also allows you to configure recording rules.

Configure the recording rule:
1. Type sum:up in the “Rule name” field.
2. Choose Cortex managed recording rule in the “Rule type” field. Make sure to select “managed recording rule” and not “managed alert rule.”
3. Choose Mimir in the “Select data source” field.
4. Type example-namespace in the “Namespace” field.
5. Type example-group in the “Group” field.
6. Type sum(up) in the “Create a query to be recorded” field.
7. From the upper-right corner, click the Save and Exit button.

Your `sum:up` recording rule will show the number of Mimir instances that are up, meaning reachable to be scraped. The rule is now being created in Grafana Mimir ruler and will be soon available for querying:

1. Open [Grafana Explore](http://localhost:9000/explore?orgId=1&left=%7B%22datasource%22:%22Mimir%22,%22queries%22:%5B%7B%22refId%22:%22A%22,%22instant%22:true,%22range%22:true,%22exemplar%22:true,%22expr%22:%22sum:up%22%7D%5D,%22range%22:%7B%22from%22:%22now-1h%22,%22to%22:%22now%22%7D%7D) and query the resulting series from the recording rule, which may require up to one minute to display after configuration:
```
sum:up
```
2. Confirm the query returns a value of `3` which is the number of Mimir instances currently running in your local setup.

## Configure an alert rule

1. Open [Grafana Alerting](http://localhost:9000/alerting/list).
2. Click to “New alert rule”.

Configure the alert rule:
1. Type `MimirNotRunning` in the “Rule name” field.
2. Choose `Cortex managed alert` rule in the “Rule type” field. Make sure to select “managed alert rule” and not “managed recording rule.”
3. Choose Mimir in the “Select data source” field.
4. Select `example-namespace` in the “Namespace” field.
5. Select `example-group` in the “Group” field.
6. Type `up == 0` in the “Create a query to be alerted on” field.
7. From the upper-right corner, click the Save and Exit button.

To see the alert firing we can introduce an outage in the Grafana Mimir cluster:

1. Abruptly terminate one of the three Grafana Mimir instances:
```
docker-compose kill mimir-3
```
2. Open [Grafana Alerting](http://localhost:9000/alerting/list) and check out the state of the alert MimirNotRunning, which should switch to “Pending” state in about one minute and to “Firing” state after another minute

1. Start the Grafana Mimir instances:
```
docker-compose start mimir-3
```
2. Open [Grafana Alerting](http://localhost:9000/alerting/list) and check out the state of the alert MimirNotRunning, which should switch to “Normal” state in about one minute.

Et voilà!



