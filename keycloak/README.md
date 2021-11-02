# Keycloak

## Install

External mode

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install keycloak-psql bitnami/postgresql -f postgresql-values.yaml
helm install keycloak bitnami/keycloak -f keycloak-values.yaml
```

## Uninstall

```bash
helm uninstall keycloak-psql
helm uninstall keycloak
```
