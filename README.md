# Enterprise EKS Cluster Management

This repository contains a complete, production-ready setup for managing an Amazon EKS (Elastic Kubernetes Service) cluster with proper security, RBAC (Role-Based Access Control), namespaces, Helm deployments, and other Kubernetes resources, all built for enterprise-level Kubernetes management.

The structure provides a robust foundation for both **development** and **production** environments with security and scalability in mind.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Folder Structure](#folder-structure)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [RBAC Configuration](#rbac-configuration)
- [Namespace Setup](#namespace-setup)
- [App Deployment](#app-deployment)
- [Security & Policies](#security-policies)
- [Useful Commands](#useful-commands)
- [Contributing](#contributing)

---

## Project Overview

This project aims to set up and manage an enterprise-level EKS cluster with best practices in mind. Key features of the setup include:

- **VPC and Subnet Creation:** Isolating resources within different subnets for security.
- **IAM Role Setup:** Proper IAM roles for EKS cluster management and worker nodes.
- **RBAC Configuration:** Fine-grained access control for developers and production teams.
- **Namespace Management:** Isolation of environments for `dev` and `prod`.
- **Security Policies:** Configuring Pod Security Policies (PSPs) and OPA Gatekeeper for compliance.
- **Helm-based App Deployment:** Efficient and scalable application deployment.

---

## Folder Structure

The folder structure for this project is as follows:

enterprise-eks-cluster-management/ ├── cluster-setup/ │ ├── eksctl-cluster.yaml # EKS Cluster configuration │ ├── iam-roles.sh # IAM role creation script │ ├── vpc-setup.sh # VPC and subnet setup script ├── namespaces/ │ ├── dev-namespace.yaml # Kubernetes Namespace for dev │ ├── prod-namespace.yaml # Kubernetes Namespace for prod ├── rbac/ │ ├── dev-role.yaml # Role for dev namespace │ ├── prod-role.yaml # Role for prod namespace │ ├── role-bindings.yaml # Role bindings for dev/prod namespaces ├── apps/ │ ├── helm-install-nginx.sh # Helm script to install NGINX │ ├── nginx-values.yaml # Values file for NGINX Helm chart ├── config/ │ ├── configmap.yaml # ConfigMap example │ ├── secret.yaml # Secret example ├── security/ │ ├── psp.yaml # PodSecurityPolicy for EKS │ ├── opa-gatekeeper-constraints.yaml # OPA Gatekeeper policies ├── README.md


---

## Prerequisites

Before setting up the EKS cluster and managing Kubernetes resources, ensure that you have the following prerequisites:

- **AWS CLI:** Ensure you have AWS CLI configured with necessary permissions.
    - Configure AWS CLI: `aws configure`
  
- **eksctl:** This tool simplifies the creation of EKS clusters.
    - Install eksctl: [Installation Guide](https://eksctl.io/)
  
- **kubectl:** A command-line tool for interacting with Kubernetes clusters.
    - Install kubectl: [Installation Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
  
- **Helm (optional):** Helm is a package manager for Kubernetes applications.
    - Install Helm: [Installation Guide](https://helm.sh/docs/intro/install/)

---

## Setup Instructions

### 1. **Create the EKS Cluster**

Follow these steps to set up an EKS cluster with `eksctl`:

1. Clone this repository:

    ```bash
    git clone https://github.com/pinkeshrepos/k8s_clustermgmt.git
    cd k8s_clustermgmt/cluster-setup
    ```

2. Edit `eksctl-cluster.yaml` to fit your desired configuration (region, node types, etc.).

3. Create the EKS cluster by running:

    ```bash
    eksctl create cluster -f eksctl-cluster.yaml
    ```

4. Wait for the cluster to be created and confirm by running:

    ```bash
    kubectl get nodes
    ```

---

### 2. **Set Up IAM Roles**

Run the `iam-roles.sh` script to create IAM roles for the EKS cluster and worker nodes.

```bash
cd cluster-setup
bash iam-roles.sh
This script creates roles with the necessary policies for your EKS environment.

3. VPC and Subnet Setup
Run the vpc-setup.sh script to create the VPC and subnets for your cluster.

bash
Copy
Edit
bash vpc-setup.sh
RBAC Configuration
1. Define Roles
dev-role.yaml: Defines access for developers in the dev namespace.

prod-role.yaml: Defines access for the production team in the prod namespace.

You can edit these files as per your team’s access needs.

2. Create RoleBindings
The role-bindings.yaml file binds the roles to users, granting access.

Apply the RBAC configurations:

bash
Copy
Edit
kubectl apply -f rbac/dev-role.yaml
kubectl apply -f rbac/prod-role.yaml
kubectl apply -f rbac/role-bindings.yaml
Namespace Setup
We define two namespaces: dev and prod.

Apply the namespace configurations:

bash
Copy
Edit
kubectl apply -f namespaces/dev-namespace.yaml
kubectl apply -f namespaces/prod-namespace.yaml
App Deployment
1. Install NGINX using Helm
Use the helm-install-nginx.sh script to install NGINX in your Kubernetes cluster.

bash
Copy
Edit
bash apps/helm-install-nginx.sh
2. Custom Helm Values
You can modify the nginx-values.yaml file to customize the Helm chart configuration before deploying.

Security Policies
1. Pod Security Policy (PSP)
The psp.yaml file defines security policies for your pods to ensure compliance.

Apply the security policies:

bash
Copy
Edit
kubectl apply -f security/psp.yaml
2. OPA Gatekeeper
The opa-gatekeeper-constraints.yaml file sets up OPA Gatekeeper for policy enforcement.

Apply the constraints:

bash
Copy
Edit
kubectl apply -f security/opa-gatekeeper-constraints.yaml
Useful Commands
Here are some useful kubectl and eksctl commands:

Check cluster status:

bash
Copy
Edit
kubectl get nodes
Check namespaces:

bash
Copy
Edit
kubectl get namespaces
List all pods in the dev namespace:

bash
Copy
Edit
kubectl get pods -n dev
View logs of a pod:

bash
Copy
Edit
kubectl logs <pod-name> -n <namespace>
Contributing
We welcome contributions! Please fork the repository, create a feature branch, and submit a pull request.

License
This project is licensed under the MIT License - see the LICENSE file for details.
