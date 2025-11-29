# EKS Course Learnings – Commands & Explanations

This document summarizes what you learned from the EKS course section, with clear explanations and ready-to-use commands.

---

## 1. Creating an EKS Cluster Using `eksctl`

`eksctl` is the simplest CLI tool to create and manage EKS clusters.

### Basic Cluster Creation

```bash
eksctl create cluster --name eks-test --nodegroup-name ng-default --node-type t3.micro --nodes 2
```

**Explanation:**

* Creates a new cluster named **eks-test**.
* Adds a node group named **ng-default**.
* Uses EC2 type **t3.micro**.
* Launches **2 nodes**.

---

### Cluster Creation With More Options

```bash
eksctl create cluster --name demoeks --version 1.14 --nodegroup-name ng-demoeks --node-type t3.micro --nodes 3 --managed
```

**Explanation:**

* Creates a cluster named **demoeks**.
* Specifies Kubernetes version **1.14**.
* Creates a **managed** node group named **ng-demoeks** (EKS-managed nodes).
* EC2 type **t3.micro**, 3 nodes.

---

## 2. Upgrading a Node Group

```bash
eksctl upgrade nodegroup --name=managed-ng-1 --cluster=managed-cluster
```

**Explanation:**

* Upgrades an existing node group (**managed-ng-1**).
* The upgrade applies to the specified EKS cluster (**managed-cluster**).

---

## 3. Installing Helm

Helm is the package manager for Kubernetes.

### Install Helm on Windows

```bash
choco install kubernetes-helm
```

**Explanation:** Installs Helm using **Chocolatey**.

---

## 4. Helm Repositories & Searches

### View Added Repositories

```bash
helm search repo
```

Lists all Helm charts available in your added repos.

### Add Bitnami Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Adds the Bitnami Helm chart repo.

### Search for Charts (Hub Search)

```bash
helm search hub nginx
```

Searches the official Artifact Hub for "nginx" charts.

---

## 5. Searching via Artifact Hub

Website: **[https://artifacthub.io/packages/helm/](https://artifacthub.io/packages/helm/)**

You can browse and find Helm charts for any Kubernetes setup.

---

## 6. Pulling a Helm Chart

```bash
helm pull bitnami/nginx --untar=true
```

**Explanation:**

* Downloads the NGINX chart from Bitnami.
* Extracts it into a folder (`--untar=true`).

---
