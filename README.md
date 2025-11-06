# 🎓 Certified Kubernetes Administrator (CKA) Learnings

Welcome to my personal learning repository — **Certified_K8S_Administrator_Learnings** 🚀  
This repo contains my notes, configurations, and hands-on practice from my journey to becoming a **Certified Kubernetes Administrator (CKA)**.

---

## 🧠 K8s Commands

```bash
kind --version
notepad config.yaml
kubectl create -f config.yaml
kubectl cluster-info
kind create cluster
kubectl get nodes
kubectl get pods
kubectl describe pod nginx-pod
kubectl exec -it nginx-pod -- sh
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl get pods nginx-pod --show-labels
kubectl get pods -o wide
kubectl get node -o wide
```

