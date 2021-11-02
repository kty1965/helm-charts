# Openldap

## Install

External mode

```bash
helm repo add helm-openldap https://jp-gouin.github.io/helm-openldap/
helm install openldap helm-openldap/openldap-stack-ha -f ./values.yaml
```

## Uninstall

```bash
helm uninstall openldap
```
