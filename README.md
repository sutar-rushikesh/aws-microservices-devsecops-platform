# 🚀 AWS Microservices DevSecOps Platform

A production-oriented **microservices e-commerce platform deployed on Amazon EKS**, implementing Infrastructure as Code, CI/CD automation, containerization, GitOps-based deployment, HTTPS, monitoring, and alerting.

The project demonstrates an end-to-end **DevSecOps workflow on AWS**, starting from source code and infrastructure provisioning through container image creation, deployment to Kubernetes, external access, and observability.

---

## 🏗️ Architecture Overview

<img width="1536" height="936" alt="Arch-dig" src="https://github.com/user-attachments/assets/ed6075be-a259-410a-aa04-71aee07addc7" />


```text
                         ┌──────────────────────┐
                         │      Developers      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       GitHub         │
                         │   Source Repository  │
                         └──────────┬───────────┘
                                    │
                         ┌──────────▼───────────┐
                         │       Jenkins        │
                         │       CI/CD           │
                         │                      │
                         │ • Checkout           │
                         │ • Build              │
                         │ • Test               │
                         │ • Security Scan      │
                         │ • Docker Build       │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │     Amazon ECR        │
                         │  Docker Image Store   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       Argo CD        │
                         │    GitOps Deploy      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                ┌──────────────────────────────────────┐
                │             Amazon EKS                │
                │                                      │
                │  ┌────────────────────────────────┐  │
                │  │        Microservices           │  │
                │  │                                │  │
                │  │ • frontend                     │  │
                │  │ • emailservice                 │  │
                │  │ • checkoutservice               │  │
                │  │ • recommendationservice        │  │
                │  │ • paymentservice                │  │
                │  │ • productcatalogservice         │  │
                │  │ • cartservice                   │  │
                │  │ • loadgenerator                 │  │
                │  │ • currencyservice               │  │
                │  │ • shippingservice               │  │
                │  │ • adservice                     │  │
                │  └────────────────────────────────┘  │
                └──────────────────┬───────────────────┘
                                   │
                 ┌─────────────────┼─────────────────┐
                 │                 │                 │
                 ▼                 ▼                 ▼
        ┌────────────────┐ ┌───────────────┐ ┌───────────────┐
        │  Prometheus    │ │    Grafana    │ │ Alertmanager  │
        │    Metrics     │ │  Dashboards   │ │    Alerts     │
        └────────────────┘ └───────────────┘ └───────────────┘
                                   │
                                   ▼
                         ┌──────────────────────┐
                         │       Route 53       │
                         │        + ACM         │
                         │       HTTPS           │
                         └──────────┬───────────┘
                                    │
                                    ▼
                              🌐 End Users
```

---

# 📌 Project Description

This project implements an AWS-based microservices e-commerce application using:

* Amazon EKS
* Kubernetes
* Docker
* Amazon ECR
* Jenkins
* Terraform
* Argo CD
* Amazon Route 53
* AWS Certificate Manager (ACM)
* Prometheus
* Grafana
* Alertmanager
* GitHub

The deployment process is automated using **Infrastructure as Code + CI/CD + GitOps**.

---

# 🎯 Project Objectives

The main objectives of this project are:

* Deploy a microservices application on Amazon EKS.
* Automate AWS infrastructure provisioning using Terraform.
* Implement CI/CD using Jenkins.
* Build and push Docker images to Amazon ECR.
* Implement GitOps deployment using Argo CD.
* Expose the application using AWS Load Balancers.
* Configure DNS using Route 53.
* Enable HTTPS using AWS Certificate Manager.
* Monitor Kubernetes infrastructure using Prometheus and Grafana.
* Configure Alertmanager for infrastructure alerts.
* Demonstrate an end-to-end DevSecOps workflow.

---

# 🧩 Microservices

The application contains the following 11 services:

| #  | Service                 |
| -- | ----------------------- |
| 1  | `emailservice`          |
| 2  | `checkoutservice`       |
| 3  | `recommendationservice` |
| 4  | `frontend`              |
| 5  | `paymentservice`        |
| 6  | `productcatalogservice` |
| 7  | `cartservice`           |
| 8  | `loadgenerator`         |
| 9  | `currencyservice`       |
| 10 | `shippingservice`       |
| 11 | `adservice`             |

Each service has its own Jenkins pipeline for building and pushing its Docker image to Amazon ECR.

---

# ☁️ AWS Infrastructure

Terraform is used to provision the AWS infrastructure.

The infrastructure includes:

* S3 bucket for Terraform state
* VPC
* Subnets
* Networking resources
* EC2 Jump Host
* IAM configuration
* Security Groups
* Amazon EKS cluster
* EKS worker/node infrastructure
* Amazon ECR repositories

The project uses an S3 backend for Terraform state management.

---

# 🏗️ Infrastructure as Code

Terraform is used for infrastructure provisioning.

### Terraform components

```text
s3-buckets/
    └── Terraform state infrastructure

terraform_main_ec2/
    └── EC2 / Jump Host infrastructure

aws-eks-terraform/
    └── EKS provisioning pipeline

aws-ecr-terraform/
    └── ECR repository provisioning
```

Terraform initialization and deployment follow the standard workflow:

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

---

# 🔄 CI/CD Pipeline

Jenkins is used as the primary CI/CD automation server.

Jenkins runs on the EC2 Jump Host.

The pipeline follows this general flow:

```text
GitHub
   │
   ▼
Jenkins
   │
   ├── Checkout Source
   │
   ├── Build Application
   │
   ├── Run Tests
   │
   ├── Security Scanning
   │
   ├── Build Docker Image
   │
   └── Push Docker Image
            │
            ▼
        Amazon ECR
```

Jenkins pipelines are configured using Jenkinsfiles stored in the repository.

---

# 🐳 Docker & Amazon ECR

Each microservice is containerized using Docker.

The Docker images are pushed to Amazon ECR.

The ECR repositories include:

```text
emailservice
checkoutservice
recommendationservice
frontend
paymentservice
productcatalogservice
cartservice
loadgenerator
currencyservice
shippingservice
adservice
```

The image lifecycle is:

```text
Source Code
     │
     ▼
Docker Build
     │
     ▼
Docker Image
     │
     ▼
Amazon ECR
     │
     ▼
Argo CD / Kubernetes
```

---

# ☸️ Amazon EKS

The application runs on an Amazon Elastic Kubernetes Service cluster.

Example cluster:

```text
EKS Cluster
    │
    ├── Kubernetes Nodes
    │
    ├── Application Namespace
    │
    ├── Argo CD Namespace
    │
    └── Monitoring Namespace
```

The application is deployed into the:

```text
dev
```

namespace.

---

# 🔁 GitOps with Argo CD

Argo CD is used to implement GitOps-based continuous deployment.

Argo CD watches the Kubernetes manifests stored in GitHub.

The deployment flow is:

```text
GitHub
   │
   │ Kubernetes manifests
   ▼
Argo CD
   │
   │ Automatic Sync
   ▼
Amazon EKS
   │
   ▼
Application Pods
```

The Kubernetes manifests are located under:

```text
kubernetes-files/
```

The Argo CD application configuration uses:

```text
Repository:
https://github.com/sutar-rushikesh/aws-microservices-devsecops-platform.git

Path:
kubernetes-files

Namespace:
dev

Sync Policy:
Automatic
```

Argo CD provides:

* Automated synchronization
* Health checks
* Deployment status
* Git-based desired state
* Rollback capability

---

# 🌐 Application Access

The application is exposed externally using AWS Load Balancing.

The external traffic flow is:

```text
User Browser
     │
     ▼
Route 53
     │
     ▼
AWS Load Balancer
     │
     ▼
Kubernetes Service
     │
     ▼
Frontend
     │
     ▼
Microservices
```

---

# 🔐 HTTPS with Route 53 & ACM

The project uses:

* Amazon Route 53 for DNS
* AWS Certificate Manager for SSL/TLS certificates
* AWS Load Balancer for HTTPS traffic

The HTTPS flow is:

```text
Browser
   │
   │ HTTPS :443
   ▼
Route 53
   │
   ▼
Load Balancer
   │
   │ ACM Certificate
   ▼
Application
```

DNS validation is used for the ACM certificate.

> **Note:** Configure your own domain name and DNS records. The domain mentioned in the project notes is environment-specific.

---

# 📊 Monitoring & Observability

The project uses the **kube-prometheus-stack** for Kubernetes monitoring.

The monitoring stack contains:

```text
Prometheus
    │
    ├── Collects Metrics
    │
    ▼
Grafana
    │
    └── Visualization / Dashboards

Alertmanager
    │
    └── Alert Notifications
```

### Monitoring Components

| Component          | Purpose                   |
| ------------------ | ------------------------- |
| Prometheus         | Metrics collection        |
| Grafana            | Monitoring dashboards     |
| Alertmanager       | Alert management          |
| Node Exporter      | Node-level metrics        |
| kube-state-metrics | Kubernetes object metrics |

---

# 📈 Grafana Dashboards

The project uses Grafana dashboards for monitoring:

* Kubernetes cluster
* Kubernetes nodes
* Kubernetes pods
* Containers
* Deployments
* Kubernetes API server
* Namespaces
* Persistent volumes
* Kubernetes networking
* Argo CD

Example dashboards referenced in the project:

```text
Kubernetes Cluster Monitoring
Kubernetes Pods / Containers
Kubernetes Deployments
Kubernetes API Server
Kubernetes Nodes
Kubernetes Namespace Monitoring
Kubernetes Persistent Volumes
Kubernetes Networking
NGINX Ingress Controller
Argo CD
```

---

# 🚨 Alerting

Alertmanager is configured for infrastructure alerts.

The project includes an example CPU alert:

```text
CPU Usage > 70%
       │
       ▼
Prometheus Rule
       │
       ▼
Alertmanager
       │
       ▼
Email Notification
```

The example alert waits for the CPU threshold to remain exceeded before triggering the notification.

SMTP configuration can be added to Alertmanager using an appropriate email provider.

---

# 🛠️ Tools & Technologies

## Cloud

* AWS
* Amazon EKS
* Amazon ECR
* Amazon EC2
* Amazon S3
* Amazon Route 53
* AWS Certificate Manager
* AWS Load Balancer

## DevOps

* Jenkins
* Argo CD
* Terraform
* Docker
* Kubernetes
* Helm
* kubectl
* AWS CLI
* eksctl

## Monitoring

* Prometheus
* Grafana
* Alertmanager
* Node Exporter
* kube-state-metrics

## Source Control

* Git
* GitHub

---

# 📂 Project Structure

A simplified project structure is:

```text
aws-microservices-devsecops-platform/
│
├── aws-eks-terraform/
│   └── eks-jenkinsfile
│
├── aws-ecr-terraform/
│   └── ecr-jenkinfile
│
├── jenkinsfiles/
│   ├── emailservice
│   ├── checkoutservice
│   ├── recommendationservice
│   ├── frontend
│   ├── paymentservice
│   ├── productcatalogservice
│   ├── cartservice
│   ├── loadgenerator
│   ├── currencyservice
│   ├── shippingservice
│   └── adservice
│
├── kubernetes-files/
│   └── Kubernetes manifests
│
├── s3-buckets/
│   └── Terraform configuration
│
├── terraform_main_ec2/
│   └── Terraform configuration
│
└── README.md
```

> The exact repository structure may contain additional files and directories not listed above.

---

# 🚀 Deployment Workflow

The complete deployment process can be summarized as:

### 1. Clone Repository

```bash
git clone https://github.com/sutar-rushikesh/aws-microservices-devsecops-platform.git
cd aws-microservices-devsecops-platform
```

### 2. Provision Terraform State Infrastructure

```bash
cd s3-buckets
terraform init
terraform plan
terraform apply -auto-approve
```

### 3. Provision EC2 / Jump Host Infrastructure

```bash
cd ../terraform_main_ec2
terraform init
terraform plan
terraform apply -auto-approve
```

### 4. Configure EKS

Use the Jenkins EKS Terraform pipeline to create the EKS infrastructure.

After the cluster is created:

```bash
aws eks --region us-east-1 update-kubeconfig --name twr-eks
```

Verify:

```bash
kubectl get nodes
kubectl get pods -A
kubectl cluster-info
```

### 5. Provision ECR

Run the ECR Terraform Jenkins pipeline.

Verify:

```bash
aws ecr describe-repositories --region us-east-1
```

### 6. Build Microservice Images

Jenkins pipelines build the Docker images for each microservice and push them to Amazon ECR.

### 7. Install Argo CD

```bash
kubectl create namespace argocd

kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify:

```bash
kubectl get pods -n argocd
```

### 8. Create Application Namespace

```bash
kubectl create namespace dev
```

### 9. Configure Argo CD

Configure the Argo CD application using:

```text
Repository:
https://github.com/sutar-rushikesh/aws-microservices-devsecops-platform.git

Path:
kubernetes-files

Namespace:
dev

Sync:
Automatic
```

### 10. Configure DNS & HTTPS

Configure:

```text
Route 53
   ↓
Load Balancer
   ↓
ACM Certificate
   ↓
HTTPS
```

### 11. Install Monitoring

Create the monitoring namespace:

```bash
kubectl create namespace monitoring
```

Add the Prometheus Helm repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Install kube-prometheus-stack:

```bash
helm install kube-prom-stack \
  prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```

Verify:

```bash
kubectl get pods -n monitoring
```

---

# 🔍 Verification Commands

## Check EKS Nodes

```bash
kubectl get nodes
```

## Check All Pods

```bash
kubectl get pods -A
```

## Check Application Namespace

```bash
kubectl get all -n dev
```

## Check Argo CD

```bash
kubectl get all -n argocd
```

## Check Monitoring

```bash
kubectl get pods -n monitoring
```

## Check ECR Repositories

```bash
aws ecr describe-repositories --region us-east-1
```

## Check Kubernetes Context

```bash
kubectl config current-context
```

---

# 🔐 Security Considerations

**Never commit credentials, access keys, GitHub tokens, passwords, API keys, or SMTP credentials to GitHub.**

Use:

* AWS IAM roles
* Jenkins Credentials
* Kubernetes Secrets
* AWS Secrets Manager where appropriate
* GitHub Secrets
* Environment variables
* Short-lived credentials

Add sensitive files to `.gitignore`.

Example:

```gitignore
.env
*.pem
*.key
*.secret
credentials
credentials.*
terraform.tfstate
terraform.tfstate.*
*.tfvars
```

### ⚠️ Important

If any AWS access key, GitHub token, Jenkins password, SMTP password, or other credential has previously been committed to a repository, **rotate/revoke it immediately** and replace it with a new credential.

---

# 🧹 Cleanup

Before destroying the environment, review all AWS resources created by Terraform and the Kubernetes deployment.

For Terraform-managed resources:

```bash
terraform destroy
```

Use this carefully and only after confirming which resources are managed by the Terraform configuration.

---

# 📚 Learning Outcomes

This project provides hands-on experience with:

* AWS cloud infrastructure
* Amazon EKS
* Kubernetes
* Docker
* Microservices architecture
* Infrastructure as Code
* Terraform
* Jenkins CI/CD
* Amazon ECR
* GitOps
* Argo CD
* Route 53
* HTTPS / ACM
* Prometheus
* Grafana
* Alertmanager
* Kubernetes monitoring
* DevSecOps practices

---

# 👨‍💻 Project Author

**Rushikesh Sutar**

AWS • DevSecOps • Kubernetes • Terraform • CI/CD • Cloud

---

# ⭐ Project Goal

The goal of this project is to demonstrate how a complete microservices application can be transformed from source code into a **cloud-native, containerized, automated, monitored, and securely accessible application running on AWS EKS**.

```text
        CODE
          │
          ▼
       GITHUB
          │
          ▼
       JENKINS
          │
          ▼
     DOCKER BUILD
          │
          ▼
        ECR
          │
          ▼
      ARGO CD
          │
          ▼
       AWS EKS
          │
          ▼
   MICROSERVICES
          │
     ┌────┴────┐
     ▼         ▼
 MONITORING   HTTPS
 Prometheus   Route 53
 Grafana      ACM
 Alertmanager
```

---

## 📌 References

* AWS
* Kubernetes
* Terraform
* Jenkins
* Argo CD
* Prometheus
* Grafana

This README is intended to document the architecture and deployment workflow of the project. Environment-specific values such as AWS account details, credentials, domain names, IP addresses, passwords, and tokens should be configured separately and must not be committed to source control.
