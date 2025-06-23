# Celebal-Devops-Assignment5  
# 🚀 Kubernetes + AKS Lab Tasks

This repository contains hands-on implementations for creating and managing Kubernetes clusters using **Minikube**, **kubeadm**, and **Azure Kubernetes Service (AKS)**.

It also includes examples of deploying microservices, configuring service types (`NodePort`, `ClusterIP`, `LoadBalancer`), and managing access and roles.

---

## 🗂️ Task Overview

| # | Task | Tool | Status |
|--|------|------|--------|
| 1 | Create a Kubernetes Cluster using Minikube | 🖥️ Local | ✅ Completed |
| 2 | Create a Kubernetes Cluster using Kubeadm | 🖥️ Virtual Machines | ✅ Completed |
| 3 | Deploy AKS Cluster via Azure Portal | ☁️ Azure Cloud | ✅ Completed |
| 4 | Deploy Microservice App on AKS | ☁️ Azure Cloud | ✅ Completed |
| 5 | Expose Services (NodePort, ClusterIP, LoadBalancer) | 🔁 K8s Services | ✅ Completed |

---

## 📁 Repository Structure
```bash
Celebal/
├── README.md
├── microservice-deployment/ 
│ └── deployment.yaml
├── exposing-services/
│ ├── clusterip.yaml
│ ├── nodeport.yaml
│ └── loadbalancer.yaml
```

---

## 1️⃣ Create Kubernetes Cluster using Minikube

🔹 **Tools Required**:
- [Minikube](https://minikube.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

🔹 **Steps**:

```bash
minikube start --driver=docker
kubectl get nodes
```
---
## 2️⃣ Create Kubernetes Cluster using Kubeadm
- 🔹 Environment: Two Ubuntu VMs (Master + Worker)
- 🔹 Key Commands:

```bash
# On Master
kubeadm init --pod-network-cidr=192.168.0.0/16

# On Worker
kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```
---
## 3️⃣ Deploy AKS Cluster using Azure Portal
- 🔹 Steps:

1. Login to Azure Portal

2. Create AKS service

3. Choose resource group, region, node count

4. Enable monitoring

5. Access cluster with Azure CLI:

```bash
az login
az aks get-credentials --resource-group <rg> --name <cluster>
kubectl get nodes
```
---
- 🔹 Role Creation:
```bash
# sample-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
---

## 4️⃣ Deploy Microservice on AKS
- 🔹 Application: Sample Store App from Azure Samples

- 🔹 Steps:
``` bash
git clone https://github.com/Azure-Samples/aks-store-demo.git
cd aks-store-demo
kubectl apply -f .
```

- 🔹 Verify Deployment:
```bash
kubectl get pods
kubectl get svc
```
---

## 5️⃣ Expose Services in Kubernetes
- 🔸 ClusterIP (internal only)
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  selector:
    app: myapp
  type: ClusterIP
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
- 🔸 NodePort (accessible via node IP and port)
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30007
```

- 🔸 LoadBalancer (external IP from cloud provider)
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  selector:
    app: myapp
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
---
## 📚 Resources Used
📘 Kubernetes Official Docs

📘 AKS Documentation

🎥 Minikube Cluster - YouTube

🎥 Kubeadm Setup - YouTube

🎥 AKS Dashboard & Monitoring - YouTube

🎥 Microservices on AKS - YouTube

🎥 Expose Services in AKS - YouTube
