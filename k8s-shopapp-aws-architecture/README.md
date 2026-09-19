# 🚀 ShopApp: High-Availability E-Commerce Infrastructure on AWS & Kubernetes

A production-grade, highly available multi-tier e-commerce architecture deployed on a self-managed Kubernetes cluster across AWS EC2 nodes.

![Architecture Diagram](docs/architecture-diagram.jpg)

---

## 📌 Architecture Overview

The system runs inside a dedicated `shop-prod` namespace:

- **Ingress Tier:** NGINX Ingress Controller handling host-based routing (`shop.company.local`).
- **Web Tier (Frontend):** 3 Replicas configured with `topologySpreadConstraints` across Worker Nodes for High Availability.
- **Backend Tier:** 2 Replicas exposed internally via `ClusterIP`.
- **Database Tier:** MySQL 8.0 with dynamic persistence using **AWS EBS CSI Driver** (`gp3` StorageClass) and secured via K8s `Secrets`.

---

## 🛠️ Tech Stack & Infrastructure

- **Cloud Provider:** AWS EC2 (1 Control Plane + 2 Worker Nodes)
- **Container Orchestration:** Kubernetes `v1.30+`
- **Dynamic Storage:** AWS EBS CSI Driver (`gp3`)
- **Ingress Controller:** NGINX Ingress
- **Security:** Kubernetes Secrets
- **Database:** MySQL 8.0

---

## 🚀 Quick Deployment Guide

Apply all Kubernetes manifests sequentially:

```bash
# 1. Namespace & Secrets
kubectl apply -f k8s/01-namespace.yaml
kubectl apply -f k8s/02-mysql-secret.yaml

# 2. Dynamic Storage & Database
kubectl apply -f k8s/03-mysql-storageclass.yaml
kubectl apply -f k8s/04-mysql-pvc.yaml
kubectl apply -f k8s/05-mysql-deployment.yaml
kubectl apply -f k8s/06-mysql-service.yaml

# 3. Application Tiers
kubectl apply -f k8s/07-backend-deployment.yaml
kubectl apply -f k8s/08-backend-service.yaml
kubectl apply -f k8s/09-web-deployment.yaml
kubectl apply -f k8s/10-web-service.yaml

# 4. Ingress Routing
kubectl apply -f k8s/11-web-ingress.yaml