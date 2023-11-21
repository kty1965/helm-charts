
# Postgres

Operator Mode

## Installation

pg-operator

```bash
kubectl create namespace postgres-system
helm repo add percona https://percona.github.io/percona-helm-charts/
helm install -n postgres-system pg-operator percona/pg-operator --wait
```
