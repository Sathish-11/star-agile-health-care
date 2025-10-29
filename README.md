# 🏥 Medicure – HealthCare CI/CD Automation Project

## 📘 Overview
**Medicure** is an end-to-end CI/CD automation project that deploys a Spring Boot healthcare microservice (“Insure Me”) on a Kubernetes cluster.  
It integrates source control, infrastructure provisioning, configuration management, containerization, continuous integration, continuous deployment, and monitoring — all automated using modern DevOps tools on AWS.

> 💡 *Note:* This setup uses a **self-managed Kubernetes cluster (kubeadm on AWS EC2 instances)** to emulate a managed EKS environment. The focus is on demonstrating cluster lifecycle management, automation, and multi-environment deployments.


---

## ⚙️ Architecture Summary
Automate the build, deployment, and monitoring of a Spring Boot–based healthcare microservice using a complete DevOps toolchain.  
The project demonstrates end-to-end CI/CD automation, from infrastructure provisioning to application monitoring, on a **self-managed Kubernetes cluster (kubeadm on AWS EC2)** that replicates production behavior similar to Amazon EKS.

1. **Terraform** provisions AWS infrastructure (VPC, EC2, IAM, networking).
2. **Ansible** configures servers and installs dependencies.
3. **Jenkins** orchestrates the CI/CD pipeline.
4. **Docker** containerizes the Spring Boot application.
5. **Kubernetes** manages multi-environment deployments (dev, stage, prod).
6. **Ingress** provides environment-based routing with domain mapping.
7. **Prometheus & Grafana** handle monitoring and visualization.

---

## 🧰 Tools & Technologies

| Category | Tools |
|-----------|--------|
| **Version Control** | Git, GitHub |
| **CI/CD** | Jenkins |
| **IaC (Infrastructure as Code)** | Terraform |
| **Configuration Management** | Ansible |
| **Containerization** | Docker |
| **Orchestration** | Kubernetes |
| **Testing** | JUnit, TestNG, Selenium |
| **Cloud Provider** | AWS (EC2, VPC, IAM, S3) |
| **Monitoring** | Prometheus, Grafana |

---

## 📂 Project Directory Structure

medicure-proj/
│
├── terraform/ # AWS infrastructure provisioning
│ ├── main.tf
│ ├── outputs.tf
│ ├── user_data.sh
│
├── ansible/ # Server configuration automation
│ ├── inventory.ini
│ ├── install-deps.yaml
│
├── k8s/ # Kubernetes manifests
│ ├── namespaces.yaml
│ ├── dev-deployment.yaml
│ ├── stage-deployment.yaml
│ ├── prod-deployment.yaml
│ ├── ingress-config.yaml
│
├── Dockerfile # Application container image definition
├── Jenkinsfile # Jenkins CI/CD pipeline script
├── pom.xml # Spring Boot project dependencies
└── README.md # Project documentation


---

## 🚀 CI/CD Workflow

### **1. Infrastructure Provisioning (Terraform)**
- Creates Jenkins EC2 instance and Kubernetes nodes.
- Configures VPC, subnets, route tables, security groups, and IAM roles.

### **2. Configuration Management (Ansible)**
- Installs Java, Maven, Docker, and essential dependencies.
- Configures SSH access and Ansible inventory for all nodes.

### **3. Continuous Integration (Jenkins)**
- Triggers automatically on code commit to GitHub.
- Builds and tests the application with Maven.
- Packages the app into a Docker image and pushes to **Docker Hub**.

### **4. Continuous Deployment (Kubernetes)**
- Jenkins deploys Docker images into **dev**, **stage**, and **prod** namespaces.
- Services are exposed using **NodePort** and **Ingress**.
- Each environment has separate deployments and URLs.

### **5. Monitoring (Prometheus & Grafana)**
- Prometheus scrapes metrics from Kubernetes nodes and Docker containers.
- Node Exporter provides system-level metrics.
- Grafana visualizes performance dashboards and alerts.

---

## 🌐 Ingress Configuration

| Environment | URL |
|--------------|-----|
| **Dev** | `http://dev.local.medicu.com` |
| **Stage** | `http://stage.local.medicu.com` |
| **Prod** | `http://local.medicu.com` |

### Add these entries to your `/etc/hosts` file:
<Public-IP> dev.local.medicu.com
<Public-IP> stage.local.medicu.com
<Public-IP> local.medicu.com


---

## 📊 Monitoring Setup

- **Prometheus** → runs on port `9090`  
- **Node Exporter** → runs on port `9100`  
- **Grafana** → runs on port `3000`  

Default Grafana credentials:

Username: admin
Password: admin

### Import the Node Exporter Dashboard:
- Navigate to **Create → Import**
- Enter Dashboard ID: **1860**
- Select Prometheus as the data source

---

## 🔁 Pipeline Execution

1. Jenkins pulls code from:
https://github.com/Sathish-11/star-agile-health-care.git

2. Jenkinsfile stages:
- **Checkout** → Pulls source code from GitHub  
- **Build & Test** → Compiles and tests using Maven  
- **Docker Build & Push** → Pushes image to Docker Hub  
- **Deploy to K8s** → Applies manifests for all namespaces  

3. Validate deployment:

kubectl get pods -A
kubectl get svc -A
kubectl get ingress -A

✅ Outputs

Automated CI/CD pipeline for end-to-end deployment

Three-environment Kubernetes setup (dev, stage, prod)

Ingress-based routing for isolated environments

Integrated monitoring with Prometheus & Grafana

Fully reproducible infrastructure using Terraform

📎 Repository
GitHub URL: https://github.com/Sathish-11/star-agile-health-care
---

✅ **Next Step:**  
Before merging your `medicure-proj` branch into `master`, make sure:
- Jenkinsfile, Terraform, and K8s YAMLs are fully tested.
- Remove temporary `.tfstate` or `.terraform/` directories.
- Then run:
  ```bash
  git checkout master
  git merge medicure-proj
  git push origin master

