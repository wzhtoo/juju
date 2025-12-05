# JUJU INFRASTRUCTURE LAB - KUBERNETES WORKLOAD ORCHESTRATION

This repository documents the foundational steps and practical deployment experience using **Juju** on top of a **MicroK8s** Kubernetes cluster.

## 1. Project Goal

The primary goal of this lab is to demonstrate proficiency in:
1.  Deploying and managing applications using the **Juju Operator Pattern**.
2.  Establishing automated **relations (integrations)** between distinct services.
3.  Maintaining an `active/idle` workload lifecycle on Kubernetes.

## 2. Lab Structure

The repository structure reflects a comprehensive learning path, from basic concepts to core automation, as shown below:

* **01_JujuFundamental**: Core concepts and Juju components.
* **02_Operation**: Principles of Juju automation.
* **03_JujuInstallation**: Setup on the local environment.
* **04_Deployment**: Practical deployment of applications and relations.

## 3. Deployment Summary (Grafana and PostgreSQL)

This lab successfully deployed Grafana (Monitoring) and PostgreSQL (Database) and established a critical relation between them:

| Application | Charm Version | Final Status | Unit Message |
| :--- | :--- | :--- | :--- |
| **grafana-k8s** | `9.5.21` | `active` | Ready |
| **postgresql-k8s** | `14.15` | `active` | `Primary` |

**Key Command for Integration:**
`juju relate grafana-k8s postgresql-k8s`

## 4. Related MicroK8s Setup

This Juju model runs on **MicroK8s** which was configured on WSL. For the setup details, installation scripts, and initial Kubernetes configurations, please refer to the supporting repository:

👉 **Supporting MicroK8s Repository:**
**`https://github.com/wzhtoo/microk8s`**
