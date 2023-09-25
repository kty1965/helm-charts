# CertManager

## Installation

### cmctl

https://cert-manager.io/docs/reference/cmctl/#install

```bash
brew install cmctl
```

### chart

> dependencies prometheus

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.13.0 \
  --set installCRDs=true \
  --set prometheus.enabled=true \  # Example: disabling prometheus using a Helm parameter
  --set webhook.timeoutSeconds=4   # Example: changing the webhook timeout using a Helm parameter
```
