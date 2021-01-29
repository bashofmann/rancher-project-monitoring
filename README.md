# POC rancher-project-monitoring

This is a proof of concept to deploy a project/namespaced based monitoring on top of rancher-monitoring v2.

## Installation

* Create a cluster
* Install monitoring V2
* Create "project-one" with a namespace "project-one-workloads" and "project-one-monitoring"
* Create "project-two" with a namespace "project-two-workloads" and "project-two-monitoring"
* Deploy test workloads
```
kubectl apply -n project-one-workloads -f test-workload/redis-prometheus-exporter.yaml
kubectl apply -n project-two-workloads -f test-workload/redis-prometheus-exporter.yaml
```
* Deploy Service Monitors, Rules and dashboards
```
kubectl apply -n project-one-workloads -f test-workload/redis-servicemonitor.yaml
kubectl apply -n project-two-workloads -f test-workload/redis-servicemonitor.yaml

kubectl apply -n project-one-workloads -f test-workload/redis-prometheus-rules.yaml
kubectl apply -n project-two-workloads -f test-workload/redis-prometheus-rules.yaml

kubectl apply -n project-one-monitoring -f test-workload/redis-grafana-dashboard.yaml
kubectl apply -n project-two-monitoring -f test-workload/redis-grafana-dashboard.yaml
```
* Adapt project ids in values yamls and install project monitoring
```
helm upgrade --install --namespace project-one-monitoring project-one-monitoring rancher-project-monitoring/ -f values-project-one.yaml
helm upgrade --install --namespace project-two-monitoring project-two-monitoring rancher-project-monitoring/ -f values-project-two.yaml
```

## Access Grafana and Prometheus

Grafana default credentials are admin/prom-operator

### project-one

Prometheus

```
kubectl -n project-one-monitoring port-forward svc/project-one-monitoring-ran-prometheus 9090
```

Grafana

```
kubectl -n project-one-monitoring port-forward svc/project-one-monitoring-grafana 8080:80
```

### project-two

Prometheus

```
kubectl -n project-two-monitoring port-forward svc/project-two-monitoring-ran-prometheus 9090
```

Grafana

```
kubectl -n project-two-monitoring port-forward svc/project-two-monitoring-grafana 8080:80
```
