# Knative

## Installation


### istio

install istio

```bash
brew install istioctl
istioctl install -y
```

knative-serving peer authenticate

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "default"
  namespace: "knative-serving"
spec:
  mtls:
    mode: PERMISSIVE
EOF
```

### serving, eventing

```bash
kubectl create ns knative-system
kubectl create namespace knative-serving
kubectl create namespace knative-eventing
kubectl label namespace knative-serving istio-injection=enabled
kubectl apply -f knative/operator-v1.9.6.yaml # above 1.10 versiob, need to kubernetes v1.26+
```
