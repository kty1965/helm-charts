# Kubernetes Cluster 사용

## Minikube

```bash
brew install minikube
```

### instakk minikube cluster

```bash
minikube start --memory 8192 --cpus 4
```

already running on minukube change config

```bash
minikube stop
minikube config set memory 8192
minikube config set cpus 4
minikube start
```

### addon

```bash
minikube addons list
minikube addons enable metrics-server
minikube dashboard
```

