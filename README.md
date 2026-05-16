# 🚀 GitOps Helm ArgoCD on AWS EKS

## 📌 Project Overview

This project demonstrates a full production-style GitOps deployment platform built on AWS using:

* Terraform Infrastructure as Code (IaC)
* AWS EKS (Elastic Kubernetes Service)
* GitHub Actions CI/CD
* Amazon ECR
* Helm Charts
* ArgoCD GitOps
* AWS Load Balancer Controller
* Route53 DNS
* AWS ACM HTTPS Certificates
* EBS CSI Driver for Persistent Storage

The platform automatically builds, pushes, and deploys a Java application to Kubernetes using GitOps workflows.

---

# 🏗️ Architecture

```text
Developer Push
       ↓
GitHub Actions CI Pipeline
       ↓
Docker Image Build
       ↓
Push Image to Amazon ECR
       ↓
Update Helm values.yaml
       ↓
ArgoCD Detects Git Change
       ↓
Helm Deployment to AWS EKS
       ↓
AWS ALB Ingress + Route53 + ACM HTTPS
       ↓
Live Kubernetes Application
```

---

# ⚙️ Technologies Used

| Category           | Technologies                 |
| ------------------ | ---------------------------- |
| Cloud              | AWS                          |
| Infrastructure     | Terraform                    |
| Kubernetes         | AWS EKS                      |
| CI/CD              | GitHub Actions               |
| GitOps             | ArgoCD                       |
| Packaging          | Helm                         |
| Containerization   | Docker                       |
| Registry           | Amazon ECR                   |
| Ingress            | AWS Load Balancer Controller |
| DNS                | Route53                      |
| SSL/TLS            | AWS ACM                      |
| Persistent Storage | EBS CSI Driver               |
| Application        | Java                         |

---

# ☁️ Infrastructure Provisioned with Terraform

The Terraform configuration provisions:

* VPC
* Public Subnets
* Internet Gateway
* Route Tables
* EKS Cluster
* EKS Node Group
* IAM Roles & Policies
* OIDC Provider for IRSA
* EKS Access Entries
* Security Groups
* EBS CSI Driver IAM Role

---

# 🔐 Security Features

* IAM Roles for Service Accounts (IRSA)
* HTTPS secured using AWS ACM certificates
* Route53 custom DNS records
* Kubernetes Secrets
* GitHub Secrets for CI/CD authentication

---

# 🔄 CI/CD Workflow

GitHub Actions pipeline performs:

1. Checkout application source code
2. Build Java application with Maven
3. Run SonarQube code analysis
4. Build Docker image
5. Push Docker image to Amazon ECR
6. Update Helm values.yaml image tag
7. Push changes to Helm GitOps repository
8. ArgoCD automatically syncs deployment to Kubernetes

---

# 📦 GitOps Workflow

ArgoCD continuously monitors the Helm GitOps repository.

Whenever GitHub Actions updates the image tag:

* ArgoCD detects the Git commit
* Automatically syncs Kubernetes manifests
* Deploys the latest application version to EKS

---

# 🌐 Ingress & HTTPS

The application is exposed publicly using:

* AWS Application Load Balancer (ALB)
* Kubernetes Ingress
* Route53 DNS
* AWS ACM SSL Certificates

Example:

```text
https://vproappgitopsargocd.tcapp.xyz
```

---

# 💾 Persistent Storage

MySQL database persistence is configured using:

* Kubernetes PersistentVolumeClaim (PVC)
* AWS EBS volumes
* EBS CSI Driver

---

# 📁 Repository Structure

## Infrastructure Repository

```text
GitOps-infracode-terraform_github-actions/
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── eks.tf
├── iam.tf
└── networking.tf
```

## Application Repository

```text
GitOps-appcode_github-actions/
├── .github/workflows/
├── Dockerfile
├── pom.xml
└── src/
```

## Helm GitOps Repository

```text
GitOps-helm-ArgoCD/
├── helm/
│   └── vproappchartsArgo/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
```

---

# 🚀 Deployment Steps

## 1. Provision Infrastructure

```bash
terraform init
terraform plan
terraform apply
```

## 2. Configure kubectl

```bash
aws eks update-kubeconfig --name vprofile-eks-cluster --region us-east-1
```

## 3. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## 4. Install AWS Load Balancer Controller

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system
```

## 5. Deploy Application via ArgoCD

```bash
argocd app create vprofile
argocd app sync vprofile
```

---

# 🛠️ Troubleshooting Performed

During implementation, several production-style issues were identified and resolved:

* ArgoCD gRPC/Ingress connectivity issues
* Helm templating errors
* YAML indentation issues
* Missing apiVersion values
* PVC binding failures
* EKS RBAC authorization problems
* Route53 DNS propagation issues
* ALB ingress configuration problems
* GitHub Actions authentication issues
* EBS CSI dynamic provisioning issues

---

# 🎯 Key Skills Demonstrated

* Kubernetes Administration
* AWS Cloud Engineering
* Terraform IaC
* GitOps Implementation
* CI/CD Automation
* Helm Chart Development
* Kubernetes Ingress & Networking
* Persistent Storage Management
* IAM & IRSA Configuration
* Troubleshooting Production Issues

---

# 📈 Future Improvements

Potential enhancements:

* Prometheus & Grafana Monitoring
* Centralized Logging (Loki / ELK)
* Horizontal Pod Autoscaler (HPA)
* Argo Rollouts
* External Secrets Operator
* Terraform Remote Backend
* Karpenter Autoscaling
* Multi-environment GitOps
* Service Mesh (Istio)

---

A production-style GitOps project demonstrating automated Kubernetes application delivery using:

- Helm Charts
- ArgoCD
- AWS EKS
- AWS Application Load Balancer (ALB)
- GitHub
- Docker
- Kubernetes
- AWS ACM SSL Certificates

This project modernizes a traditional multi-tier Java application into a cloud-native GitOps deployment workflow using declarative Kubernetes infrastructure and continuous delivery principles. GitOps uses Git as the single source of truth for Kubernetes deployments. :contentReference[oaicite:0]{index=0}

---

# Architecture

```text
Developer Push
      ↓
GitHub Repository
      ↓
ArgoCD Watches Repository
      ↓
Helm Charts Render Kubernetes Manifests
      ↓
AWS EKS Cluster
      ↓
AWS Load Balancer Controller
      ↓
AWS ALB + ACM SSL
      ↓
Application Deployment
````

---

# Technologies Used

| Technology         | Purpose                       |
| ------------------ | ----------------------------- |
| Kubernetes         | Container orchestration       |
| Helm               | Kubernetes package management |
| ArgoCD             | GitOps continuous delivery    |
| AWS EKS            | Managed Kubernetes cluster    |
| AWS ALB Controller | Native AWS ingress controller |
| Docker             | Containerization              |
| GitHub             | GitOps source repository      |
| Route53            | DNS management                |
| AWS ACM            | SSL certificate management    |

---

# Features

* GitOps deployment model
* Reusable Helm chart architecture
* Parameterized Kubernetes manifests
* AWS-native ALB ingress integration
* SSL/TLS with AWS ACM
* Kubernetes Secrets management
* Persistent storage with PVC
* Automated application synchronization using ArgoCD
* Environment-ready Helm values structure
* Production-style Kubernetes deployments

---

# Repository Structure

```text
GitOps-helm-ArgoCD/
│
├── helm/
│   └── vproappchartsArgo/
│       ├── templates/
│       ├── Chart.yaml
│       └── values.yaml
│
├── kubedefs/
│   └── Raw Kubernetes manifests
│
└── README.md
```

---

# Helm Chart Components

The Helm chart deploys:

* Application Deployment
* MySQL Database
* RabbitMQ
* Memcached
* Services
* Ingress
* Persistent Volume Claims
* Kubernetes Secrets

---

# Helm Features

The chart is fully parameterized using `values.yaml`.

Example:

```yaml
app:
  image: tcollins520/vprofileapp
  tag: latest

ingress:
  host: vproappgitopsargocd.tcapp.xyz
```

This allows:

* environment-specific overrides
* reusable deployments
* GitOps automation
* scalable configuration management

---

# AWS ALB Ingress

This project uses the AWS Load Balancer Controller instead of NGINX Ingress.

Benefits include:

* Native AWS integration
* Automatic ALB provisioning
* ACM SSL integration
* Route53 compatibility
* Simplified ingress management

Example ingress annotations:

```yaml
alb.ingress.kubernetes.io/scheme: internet-facing
alb.ingress.kubernetes.io/target-type: ip
alb.ingress.kubernetes.io/ssl-redirect: '443'
```

---

# Prerequisites

Before deployment, install:

* kubectl
* eksctl
* Helm
* AWS CLI
* Docker

You must also configure:

* AWS credentials
* EKS cluster
* Route53 hosted zone
* AWS ACM certificate

---

# Create EKS Cluster

Example:

```bash
eksctl create cluster \
--name vproapp-argocd-eks \
--region us-east-1 \
--nodegroup-name workers \
--node-type t3.small \
--nodes 2
```

---

# Install AWS Load Balancer Controller

Associate IAM OIDC provider:

```bash
eksctl utils associate-iam-oidc-provider \
--region us-east-1 \
--cluster vproapp-argocd-eks \
--approve
```

Install the controller using Helm.

---

# Install ArgoCD

Create namespace:

```bash
kubectl create namespace argocd
```

Install ArgoCD:

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

ArgoCD is a declarative GitOps continuous delivery tool for Kubernetes. ([GitHub][1])

---

# Deploy Application with Helm

Validate chart:

helm lint .
```

Render templates:

```bash
helm template vproapprelease .
```

Deploy chart:

helm upgrade --install vproapprelease .
```

---

# GitOps Workflow

1. Developer pushes changes to GitHub
2. ArgoCD detects repository changes
3. Helm renders Kubernetes manifests
4. ArgoCD synchronizes cluster state
5. Application updates automatically

This follows GitOps principles where Git acts as the single source of truth for infrastructure and application state. ([OneUptime][2])

---

# 👩‍💻 Author

Tina Collins

DevOps Engineer | AWS | Kubernetes | Terraform | GitOps | CI/CD
