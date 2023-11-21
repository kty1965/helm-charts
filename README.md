# Kubernetes Cluster 사용

## Minikube

```bash
brew install minikube
```

### instakk minikube cluster

```bash
minikube start --memory 4096 --cpus 2 --nodes 2
```

already running on minukube change config

```bash
minikube stop
minikube config set memory 4096
minikube config set cpus 2
minikube start
```

### addon

```bash
minikube addons enable metrics-server
minikube addons enable storage-provisioner
minikube addons enable volumesnapshots
minikube addons enable default-storageclass
minikube addons enable csi-hostpath-driver
minikube dashboard
```
