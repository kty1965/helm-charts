# Direktiv

## Installation

### Direktiv Minikube cluster setup

```bash
minikube start --memory=4096 --cpus=2 --disk-size=30g --kubernetes-version=v1.25.14 --nodes 3 --profile direktiv
minikube addons enable -p direktiv metrics-server
minikube addons disable -p direktiv storage-provisioner
minikube addons disable -p direktiv default-storageclass
# minikube addons disalbe -p direktiv volumesnapshots
# minikube addons disalbe -p direktiv csi-hostpath-driver
# kubectl patch storageclass csi-hostpath-sc -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
minikube dashboard
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: kubevirt.io/hostpath-provisioner
volumeBindingMode: WaitForFirstConsumer
reclaimPolicy: Delete
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: kubevirt-hostpath-provisioner
subjects:
- kind: ServiceAccount
  name: kubevirt-hostpath-provisioner-admin
  namespace: kube-system
roleRef:
  kind: ClusterRole
  name: kubevirt-hostpath-provisioner
  apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: kubevirt-hostpath-provisioner
rules:
  - apiGroups: [""]
    resources: ["nodes"]
    verbs: ["get"]
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "update"]

  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch"]

  - apiGroups: [""]
    resources: ["events"]
    verbs: ["list", "watch", "create", "update", "patch"]
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: kubevirt-hostpath-provisioner-admin
  namespace: kube-system
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: kubevirt-hostpath-provisioner
  labels:
    k8s-app: kubevirt-hostpath-provisioner
  namespace: kube-system
spec:
  selector:
    matchLabels:
      k8s-app: kubevirt-hostpath-provisioner
  template:
    metadata:
      labels:
        k8s-app: kubevirt-hostpath-provisioner
    spec:
      serviceAccountName: kubevirt-hostpath-provisioner-admin
      containers:
        - name: kubevirt-hostpath-provisioner
          image: quay.io/kubevirt/hostpath-provisioner
          imagePullPolicy: Always
          env:
            - name: USE_NAMING_PREFIX
              value: "false" # change to true, to have the name of the pvc be part of the directory
            - name: NODE_NAME
              valueFrom:
                fieldRef:
                  fieldPath: spec.nodeName
            - name: PV_DIR
              value: /tmp/hostpath-provisioner
          volumeMounts:
            - name: pv-volume # root dir where your bind mounts will be on the node
              mountPath: /tmp/hostpath-provisioner/
              #nodeSelector:
              #- name: xxxxxx
      volumes:
        - name: pv-volume
          hostPath:
            path: /tmp/hostpath-provisioner/
EOF
```

### Database(Postgres)

### Knative

```bash
kubectl create ns knative-system
kubectl create namespace knative-serving
kubectl create namespace knative-eventing
kubectl apply -f knative/operator-v1.9.6.yaml # above 1.10 versiob, need to kubernetes v1.26+
kubectl apply -f contour/net-contour-v1.9.3.yaml
kubectl apply -f knative/serving-v1.9.6.yaml
# kubectl delete namespace contour-external
```

### Direktiv

```bash
kubectl create namespace direktiv-system
kubectl label namespace direktiv-system
helm install -f values.yaml -n direktiv-system direktiv direktiv/direktiv
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: dnsutils
  namespace: direktiv-system
spec:
  containers:
  - name: dnsutils
    image: registry.k8s.io/e2e-test-images/jessie-dnsutils:1.3
    command:
      - sleep
      - "infinity"
    imagePullPolicy: IfNotPresent
  restartPolicy: Always
EOF
```
