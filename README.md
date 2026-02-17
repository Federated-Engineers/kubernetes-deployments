# kubernetes-deployments

This repository contains all the Kubernetes deployment configurations required to provision and manage our core data applications such as airflow and airbyte on Amazon EKS.

The goal of this repository is simple:

Define infrastructure and application deployments declaratively and manage them using GitOps principles.

Everything here is structured to support a clean, production-ready deployment flow using Argo CD as the GitOps controller.

## What This Repository Represents

This repository serves as:
- The source of truth for Kubernetes application deployments
- The GitOps-managed configuration store for EKS workloads
- A centralized place to manage infrastructure-level and application-level manifests

All Kubernetes manifests and Helm configurations required to deploy the following services are included:
- Airflow
- Airbyte
- Argo CD

The repository is structured intentionally to separate application deployments from Argo CD bootstrap logic.

## Repository Structure
```
.
├── .github/
├── applications/
│   ├── airbyte/
│   ├── airflow/
│   └── storage-class.yaml
├── argocd-bootstrap/
│   ├── argo-applications/
│   │   ├── airbyte.yaml
│   │   └── airflow.yaml
│   └── argo-cd-chart/
├── .gitignore
└── README.md
```

## applications/

This directory contains the actual Kubernetes deployment configurations for platform applications.
Each subfolder represents a deployable workload on EKS.

### airbyte/
This folder contains all manifests and configuration required to deploy Airbyte into the EKS cluster.
Everything required for Airbyte to run inside the cluster lives here.
Argo CD will point directly to this folder when managing the Airbyte deployment.

### airflow/

This folder contains all manifests and configurations required to deploy Apache Airflow into EKS.
This structure makes Airflow deployment self-contained and version controlled.

### storage-class.yaml

This file defines the Kubernetes StorageClass used within the cluster.
In EKS, this is typically backed by:
- EBS (gp3)

This ensures persistent workloads (Airflow logs, Airbyte state, etc.) have properly provisioned storage.
Keeping it inside applications/ ensures that application storage requirements are defined alongside workloads.

### argocd-bootstrap/

This directory handles bootstrapping Argo CD and defining what Argo CD should manage.
There is a clear separation here:
- argo-cd-chart/ : deploy Argo CD itself
- argo-applications/ : tell Argo CD what to manage
This separation keeps the control plane clean.

