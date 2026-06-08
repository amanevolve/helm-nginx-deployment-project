# Helm Nginx Deployment Project

## Project Overview

This project demonstrates how to deploy a custom Nginx web application on Kubernetes using Helm Charts.

The project includes:

* Docker image creation
* Docker Hub integration
* Kubernetes Deployment
* Kubernetes Service
* Helm Chart creation
* Helm Upgrade process
* AWS EC2 hosting

---

## Architecture

Developer → Docker Image → Docker Hub → Helm Chart → Kubernetes Cluster → Nginx Application

---

## Technologies Used

* Docker
* Docker Hub
* Kubernetes
* Helm
* AWS EC2 (Ubuntu)
* Nginx

---

## Project Structure

```text
helm-nginx-project/
│
├── helm-website/
│   ├── Dockerfile
│   └── index.html
│
├── nginx-app/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
│       ├── deployment.yaml
│       └── service.yaml
│
└── README.md
```

---

# Step 1: Create Custom Website

Create index.html

```bash
nano index.html
```

Create Dockerfile

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html
```

---

# Step 2: Build Docker Image

```bash
docker build -t aman4255/helm-nginx:v2 .
```

Verify:

```bash
docker images
```

---

# Step 3: Login to Docker Hub

```bash
docker login
```

---

# Step 4: Push Image to Docker Hub

```bash
docker push aman4255/helm-nginx:v2
```

---

# Step 5: Create Helm Chart

```bash
helm create nginx-app
```

Remove unnecessary files and keep:

```text
templates/
├── deployment.yaml
└── service.yaml
```

---

# Step 6: Validate Helm Chart

```bash
helm lint ./nginx-app
```

Render templates:

```bash
helm template nginx-release ./nginx-app
```

---

# Step 7: Install Helm Chart

```bash
helm install nginx-release ./nginx-app \
-n nginx-app-ns \
--create-namespace
```

---

# Step 8: Verify Deployment

```bash
kubectl get all -n nginx-app-ns
```

Check pods:

```bash
kubectl get pods -n nginx-app-ns
```

Check services:

```bash
kubectl get svc -n nginx-app-ns
```

---

# Step 9: Upgrade Helm Release

Update values.yaml:

```yaml
image:
  repository: aman4255/helm-nginx
  tag: v2
```

Upgrade:

```bash
helm upgrade nginx-release ./nginx-app -n nginx-app-ns
```

Restart deployment:

```bash
kubectl rollout restart deployment nginx-release -n nginx-app-ns
```

---

# Step 10: Access Application

```bash
kubectl port-forward --address 0.0.0.0 svc/nginx-svc 9090:81 -n nginx-app-ns
```

Open:

```text
http://EC2-PUBLIC-IP:9090
```

---

# Useful Commands

List Helm releases:

```bash
helm list -A
```

Uninstall release:

```bash
helm uninstall nginx-release -n nginx-app-ns
```

Delete namespace:

```bash
kubectl delete namespace nginx-app-ns
```

---

# Learning Outcomes

* Helm Chart Development
* Docker Image Management
* Kubernetes Deployments
* Kubernetes Services
* Docker Hub Integration
* AWS EC2 Administration
* Helm Release Lifecycle Management

---

Created By Aman Kumar
