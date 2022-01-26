# Kafka

```plantext
used chart: https://github.com/bitnami/charts/tree/master/bitnami/kafka
kafka version: 2.6.x
chart version: 11.8.9
```

## Install

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install --version 11.8.9 --create-namespace --namespace kafka-system -f values.yaml kafka bitnami/kafka
```
