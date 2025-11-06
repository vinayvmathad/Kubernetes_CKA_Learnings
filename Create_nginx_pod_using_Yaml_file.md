# Creating Nginx pod through imperative and declarative way.

This document contains all Kubernetes commands and outputs executed while practicing on Windows PowerShell using **Kind** (Kubernetes in Docker).

---

## 🧱 Cluster Setup and Configuration

```bash
# Check Kind version
kind --version

# Create and edit config file
notepad config.yaml

# Attempt to create Kubernetes resources from config
kubectl create -f config.yaml

# List directory contents
ls

# Check cluster info (initially failed because cluster not created)
kubectl cluster-info

# Create a Kind cluster
kind create cluster

# Verify cluster nodes
kubectl get nodes
```

---

## 🚀 Pod Creation and Management

```bash
# Create a Pod from YAML
kubectl create -f config.yaml

# Check pod status
kubectl get pods

# Describe pod details
kubectl describe pod

# Access running container shell
kubectl exec -it nginx-pod -- sh

# Exit container shell
exit

# Edit pod (no changes made)
kubectl edit pod nginx-pod
```

---

## 🧾 Pod Generation Using Dry Run

```bash
# Dry run to generate Pod definition
kubectl run nginx --image=nginx --dry-run=client

# Attempt to output YAML (typo example)
kubectl run nginx --image=nginx --dry-run=client -yaml

# Correct YAML output command
kubectl run nginx --image=nginx --dry-run=client -o yaml

# Save YAML to file
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

---

## 🔍 Inspecting and Label Management

```bash
# Describe the existing Pod again
kubectl describe pod nginx-pod

# Show pod labels
kubectl get pods nginx-pod --show-labels

# Display detailed pod info
kubectl get pods -o wide

# Display node details
kubectl get node -o wide

# Verify nodes again
kubectl get nodes
```

---

## ⚠️ Common Errors Encountered

```bash
# API server not reachable before Kind cluster was started
error: error validating "config.yaml": failed to download openapi: Get "http://localhost:8080/openapi/v2?timeout=32s": dial tcp [::1]:8080

# Typo errors
kubectle create config.yaml        # Incorrect spelling (should be kubectl)
kubectl decribe pod                # Typo (should be describe)

# Invalid flags
kubectl exec -it nginx-pod --sh    # Incorrect flag usage
kubectl get pods --o wide          # Incorrect flag (--o instead of -o)
```

---

## ✅ Key Learnings

* **Kind** creates a lightweight Kubernetes cluster locally.
* Common debugging steps:

  * Verify API server connectivity using `kubectl cluster-info`.
  * Inspect pod details with `kubectl describe`.
  * Access containers interactively via `kubectl exec -it <pod> -- sh`.
* **Dry-run mode** (`--dry-run=client -o yaml`) is helpful for generating YAML manifests.
* **Labels** (`--show-labels`) help categorize and manage pods.

---

## 📂 Files Created

* `config.yaml` – Pod configuration YAML file.
* `pod.yaml` – Generated pod manifest via dry-run.
* `Create simple pod.md` – Simple pod creation reference.
* `Kind_Kubernetes_on_Windows.md` – Kind setup guide.
* `README.md` – Repository overview.

---


# Certified Kubernetes Administrator (CKA) Practice - PowerShell Session

This document contains all Kubernetes commands **along with outputs** executed while practicing on Windows PowerShell using **Kind (Kubernetes in Docker)**.

---

## 🧱 Cluster Setup and Configuration

```bash
PS C:\Users\USER\Kubernetes_CKA_Learnings> kind --version
kind version 0.30.0

PS C:\Users\USER\Kubernetes_CKA_Learnings> notepad config.yaml

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl create -f config.yaml
error: error validating "config.yaml": error validating data: failed to download openapi: Get "http://localhost:8080/openapi/v2?timeout=32s": dial tcp [::1]:8080: connectex: No connection could be made because the target machine actively refused it.

PS C:\Users\USER\Kubernetes_CKA_Learnings> ls

    Directory: C:\Users\USER\Kubernetes_CKA_Learnings

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         11/6/2025   5:25 PM            192 config.yaml
-a----         11/6/2025   5:06 PM            260 Create simple pod.md
-a----         11/6/2025   5:06 PM           5037 Kind_Kubernetes_on_Windows.md
-a----         11/6/2025   5:06 PM            309 README.md

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl cluster-info
E1106 19:00:02.975887   28324 memcache.go:265] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connectex: No connection could be made because the target machine actively refused it.
Unable to connect to the server: dial tcp [::1]:8080: connectex: No connection could be made because the target machine actively refused it.

PS C:\Users\USER\Kubernetes_CKA_Learnings> kind create cluster
Creating cluster "kind" ...
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get nodes
NAME                 STATUS   ROLES           AGE     VERSION
kind-control-plane   Ready    control-plane   4m34s   v1.34.0
```

---

## 🚀 Pod Creation and Management

```bash
PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl create -f config.yaml
pod/nginx-pod created

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get pods
NAME        READY   STATUS              RESTARTS   AGE
nginx-pod   0/1     ContainerCreating   0          15s

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          2m41s

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl describe pod nginx-pod
Name:             nginx-pod
Namespace:        default
Status:           Running
Node:             kind-control-plane/172.18.0.2
IP:               10.244.0.5
Containers:
  nginx-container:
    Image:          nginx
    State:          Running
    Ready:          True
Events:
  Normal  Scheduled  default-scheduler  Successfully assigned default/nginx-pod to kind-control-plane
  Normal  Pulling    kubelet            Pulling image "nginx"
  Normal  Started    kubelet            Started container nginx-container
```

---

## 🧾 Pod Interaction

```bash
PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl exec -it nginx-pod --sh
error: unknown flag: --sh
See 'kubectl exec --help' for usage.

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl exec -it nginx-pod -- sh
# pwd
/
# ls -lrt
total 68
lrwxrwxrwx   1 root root    8 Aug 24 16:20 sbin -> usr/sbin
...
-rw-r--r--   1 root root   37 Nov  6 15:51 product_uuid
drwxr-xr-x   1 root root 4096 Nov  6 15:51 run
# exit
```

---

## 🧩 Pod Generation Using Dry Run

```bash
PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl run nginx --image=nginx --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

---

## 🔍 Inspecting and Label Management

```bash
PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get pods nginx-pod --show-labels
NAME        READY   STATUS    RESTARTS   AGE   LABELS
nginx-pod   1/1     Running   0          36m   env=demo,type=frontend

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get pods -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP           NODE                 NOMINATED NODE   READINESS GATES
nginx-pod   1/1     Running   0          38m   10.244.0.5   kind-control-plane   <none>           <none>

PS C:\Users\USER\Kubernetes_CKA_Learnings> kubectl get node -o wide
NAME                 STATUS   ROLES           AGE    VERSION   INTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                     CONTAINER-RUNTIME
kind-control-plane   Ready    control-plane   168m   v1.34.0   172.18.0.2    Debian GNU/Linux 12 (bookworm)   6.6.87.2-microsoft-standard-WSL2   containerd://2.1.3
```

---

## ⚠️ Common Errors Encountered

```bash
error: error validating "config.yaml": failed to download openapi: Get "http://localhost:8080/openapi/v2?timeout=32s": dial tcp [::1]:8080: connectex: No connection could be made
kubectle create config.yaml        # Typo (kubectle instead of kubectl)
kubectl decribe pod                # Typo (decribe instead of describe)
kubectl exec -it nginx-pod --sh    # Incorrect flag usage
kubectl get pods --o wide          # Incorrect flag (--o instead of -o)
```
