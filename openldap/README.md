# Openldap

## Install

External mode

```bash
helm repo add helm-openldap https://jp-gouin.github.io/helm-openldap/
helm install openldap helm-openldap/openldap-stack-ha -f ./values.yaml -n openldap-system --create-namespace
kubectl label namespace openldap-system istio-injection=enabled
```

## Uninstall

```bash
helm uninstall openldap
```
