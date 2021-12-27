# Argo CD

## Installation

### Secret

```bash
kubectl apply -f ./k8s
```

### Redis

install

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install --create-namespace --namespace argo argo-redis bitnami/redis -f redis-values.yaml
```

uninstall

```bash
helm uninstall --namespace argo argo-redis
```

check

```bash
export REDIS_PASSWORD=$(kubectl get secret --namespace argo argo-redis -o jsonpath="{.data.redis-password}" | base64 --decode)
echo $REDIS_PASSWORD
```

### ArgoCD

prerequesits

Check redis password

install

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install --create-namespace --namespace argo argo-cd bitnami/argo-cd  -f values.yaml
```

uninstall

```bash
helm uninstall --namespace argo argo-redis
helm uninstall --namespace argo argo-cd
```

1. Access your Argo CD installation:
  Execute the following commands:

    ```bash
      kubectl port-forward --namespace argo svc/argo-cd-server 8080:80 &
      export URL=http://127.0.0.1:8080/
      echo "Argo CD URL: http://127.0.0.1:8080/"
    ```

1. Execute the following commands to obtain the Argo CD credentials:

   ```bash
     echo "Username: \"admin\""
     echo "Password: $(kubectl -n argo get secret argocd-secret -o jsonpath="{.data.clearPassword}" | base64 -d)"
   ```
