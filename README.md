# Grafana agent operator for K8s

## Description
[Grafana Agent](https://github.com/grafana/agent) is a telemetry collector for sending metrics, logs, and trace data to the opinionated
Grafana observability stack.

## Usage

Create a Juju model for your operators, say "lma"

```bash
    juju add-model lma
```

The Grafana agent may be deployed using the juju command line:

```bash
juju deploy grafana-agent-k8s
```

If required, you can remove the deployment completely:

```bash
    juju destroy-model -y lma --no-wait --force --destroy-storage
```

## Relations

Currently supported relations are:


### Required side

- [prometheus-remote-write](https://github.com/canonical/prometheus-operator/): to be used to push data to a charm exposing the [Prometheus](https://grafana.com/oss/prometheus/) `prometheus_remote_write` API.
- [metrics-endpoint](https://charmhub.io/prometheus-k8s/libraries/prometheus_scrape): to be used to integrate with charms that exposes metrics using the `prometheus_scrape` API.
- [logging](https://charmhub.io/loki-k8s/libraries/loki_push_api): to be used to integrate with a charms that provides a `loki_push_api` interface, for instance the [Loki charmed operator](https://grafana.com/oss/loki/). Through this relation, the Grafana agent charm will send labeled log generated by a workload charm.


### Provider side:

  - [log_proxy](https://charmhub.io/loki-k8s/libraries/log_proxy): to be used as a log proxy between a workload charm and a charms that provides a `loki_push_api` interface, for instance the [Loki charmed operator](https://grafana.com/oss/loki/). When this relation is established, a [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) binary is injected and configured into the workload charm. This Promtail binary will label and send the logs to this charm which in turn will redirect them to Loki.


## OCI Images

This charm by default uses the latest release of the
[grafana/agent](https://hub.docker.com/r/grafana/agent)
