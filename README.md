````markdown
# GitOps Helm ArgoCD on AWS EKS

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

```bash
helm lint .
```

Render templates:

```bash
helm template vproapprelease .
```

Deploy chart:

```bash
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

# Future Improvements

* Multi-environment deployments
* ArgoCD ApplicationSets
* Sealed Secrets / External Secrets
* GitHub Actions CI integration
* Image automation
* Monitoring with Prometheus & Grafana
* Horizontal Pod Autoscaling
* Production-grade observability

---

# Learning Objectives

This project demonstrates:

* Kubernetes application modernization
* Helm templating
* GitOps workflows
* AWS-native Kubernetes ingress
* Declarative infrastructure management
* Cloud-native CI/CD practices
* Production-style EKS deployments

---

# References

* [ArgoCD Official Documentation](https://argo-cd.readthedocs.io?utm_source=chatgpt.com)
* [Helm Documentation](https://helm.sh/docs?utm_source=chatgpt.com)
* [AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest?utm_source=chatgpt.com)
* [Amazon EKS Documentation](https://docs.aws.amazon.com/eks?utm_source=chatgpt.com)

---

```
```

[1]: https://github.com/argoproj/argo-cd?utm_source=chatgpt.com "argoproj/argo-cd: Declarative Continuous Deployment for ..."
[2]: https://oneuptime.com/blog/post/2026-01-17-helm-argocd-gitops-deployment/view?utm_source=chatgpt.com "Helm + ArgoCD GitOps Deployment Complete Guide"
