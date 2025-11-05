# 🐳 Install Docker Desktop and Setup Kind Kubernetes on Windows

## 1️⃣ Install Docker Desktop

Download and install **Docker Desktop for Windows** from  
👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

Once installed, make sure:

- Docker is running (you can see the whale 🐳 icon in the system tray)
- Run the following command to verify Docker installation:

```powershell
docker -v
```

---

## 2️⃣ Install Kind (Kubernetes in Docker)

Run the following commands in **Windows PowerShell**:

```powershell
# Download the Kind binary
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.30.0/kind-windows-amd64
```

Move it to a folder in your PATH (adjust the path if needed):

```powershell
Move-Item .\kind-windows-amd64.exe "C:\Program Files\Docker\kind.exe"
```

If the above path gives permission issues, use:

```powershell
Move-Item ".\kind-windows-amd64.exe" "$env:USERPROFILE\kind.exe"
setx PATH "$($env:PATH);$env:USERPROFILE"
```

Now verify installation:

```powershell
kind --version
```

✅ Expected output:
```
kind version 0.30.0
```

---

## 3️⃣ Create a Single Node Kind Cluster

```powershell
kind create cluster --name=single-node-demo
```

**Example Output:**

```
Creating cluster "single-node-demo" ...
• Ensuring node image (kindest/node:v1.34.0) 🖼  ...
✓ Ensuring node image (kindest/node:v1.34.0) 🖼
• Preparing nodes 📦   ...
✓ Preparing nodes 📦
• Writing configuration 📜  ...
✓ Writing configuration 📜
• Starting control-plane 🕹️  ...
✓ Starting control-plane 🕹️
• Installing CNI 🔌  ...
✓ Installing CNI 🔌
• Installing StorageClass 💾  ...
✓ Installing StorageClass 💾
Set kubectl context to "kind-single-node-demo"
```

---

## 4️⃣ Verify Cluster and Context

```powershell
kubectl cluster-info --context kind-single-node-demo
kind get clusters
kubectl config current-context
```

✅ Example output:
```
single-node-demo
kind-single-node-demo
```

---

## 5️⃣ Create a Multi-Node Cluster (Optional)

Create a file named `multi-node-cluster.yaml` and paste the following:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
```

Now run:

```powershell
kind create cluster --config multi-node-cluster.yaml --name multi-node
```

Check the nodes:

```powershell
kubectl get nodes
```

✅ Example output:
```
NAME                       STATUS   ROLES           AGE     VERSION
multi-node-control-plane   Ready    control-plane   3m35s   v1.34.0
multi-node-worker          Ready    <none>          3m18s   v1.34.0
multi-node-worker2         Ready    <none>          3m20s   v1.34.0
```

---

## 6️⃣ Deploy and Test NGINX

Create a simple NGINX deployment:

```powershell
kubectl create deployment nginx --image=nginx
kubectl get pods
```

✅ Example output:
```
NAME                     READY   STATUS    RESTARTS   AGE
nginx-66686b6766-9lvcb   1/1     Running   0          25s
```

---

## 7️⃣ Port Forward NGINX to Localhost

Forward traffic from your local machine to the NGINX pod:

```powershell
kubectl port-forward pod/nginx-66686b6766-9lvcb 8080:80
```

Now open your browser and visit:

👉 [http://localhost:8080](http://localhost:8080)

You should see the **NGINX welcome page**.

---

## 8️⃣ View Running Clusters and Containers

```powershell
kind get clusters
docker ps
```

✅ Example output:
```
CONTAINER ID   IMAGE                  STATUS          PORTS
0cc523d1c083   kindest/node:v1.34.0   Up 5 minutes    127.0.0.1:59982->6443/tcp   multi-node-control-plane
b32a60fcb801   kindest/node:v1.34.0   Up 16 minutes   127.0.0.1:59781->6443/tcp   single-node-demo-control-plane
```

---

## 🧹 9️⃣ Cleanup (Optional for Learning)

When you’re done testing, delete the clusters and clean up Docker resources:

```powershell
kind delete cluster --name multi-node
kind delete cluster --name single-node-demo
docker container prune -f
docker image prune -a -f
```

Verify cleanup:

```powershell
kind get clusters
docker ps
```

✅ Output should show no clusters or running Kind containers.

---

## ✅ Summary

| Step | Description | Command |
|------|--------------|----------|
| 1 | Install Docker Desktop | `docker -v` |
| 2 | Install Kind | `kind --version` |
| 3 | Create Cluster | `kind create cluster` |
| 4 | Verify Cluster | `kubectl get nodes` |
| 5 | Deploy NGINX | `kubectl create deployment nginx --image=nginx` |
| 6 | Port Forward | `kubectl port-forward pod/nginx... 8080:80` |
| 7 | Cleanup | `kind delete cluster --name <name>` |

---

🎉 **Congratulations!**  
You’ve successfully installed Docker, Kind, created Kubernetes clusters, deployed an app, and verified end-to-end connectivity on Windows 🚀
