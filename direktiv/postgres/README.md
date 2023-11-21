
# Postgres

Operator Mode

## Installation

### Database

```bash
kubectl create namespace postgres-system
helm repo add percona https://percona.github.io/percona-helm-charts/
helm install -n postgres-system pg-operator percona/pg-operator --wait
```

### Knative

```bash
kubectl apply -f https://github.com/knative/operator/releases/download/knative-v1.9.4/operator.yaml
```
