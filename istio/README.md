# Istio

```bash
istioctl install --set profile=default # use prod
istioctl install --set profile=demo # use demo
```

## Inject istio-proxy

```bash
kubectl label namespace default istio-injection=enabled
```

## Sample bookinfo install

```bash
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

## Kiali install

```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.12/samples/addons/kiali.yaml
```

## Prometheus install

```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.12/samples/addons/prometheus.yaml
```
