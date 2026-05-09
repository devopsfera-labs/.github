# 🛠️ DevOpsfera Labs

<div align="center">
  <img src="https://img.shields.io/badge/Maintained%20by-Leandro%20Barbosa-blue?style=flat-square&logo=github" alt="Maintained by Leandro Barbosa">
  <img src="https://img.shields.io/badge/Focus-Platform%20Engineering-black?style=flat-square" alt="Focus: Platform Engineering">
  <img src="https://img.shields.io/badge/Core%20Project-FlowDX-orange?style=flat-square" alt="Core Project: FlowDX">
</div>

## About

**DevOpsfera Labs** is my engineering workspace for DevOps, Cloud Native, Platform Engineering, GitOps, automation, and Developer Experience (DX).

The main project in this organization is **FlowDX**, a Platform Engineering initiative focused on standardizing workload delivery through reusable workflows, declarative contracts, GitOps, Policy-as-Code, and governed automation.

FlowDX is not intended to be a complete Internal Developer Platform at this stage. The current focus is to build a practical platform foundation for self-service workload delivery.

## Core Project: FlowDX

**FlowDX** explores how application repositories can consume standardized platform capabilities instead of rebuilding CI/CD, delivery, governance, and Kubernetes deployment logic from scratch.

The reference architecture is based on:

- `flowdx-platform`: contracts, reusable workflows, templates, and policies
- `flowdx-reference-api`: FastAPI workload with Docker, Helm, structured logs, and `flowdx.yaml`
- `flowdx-gitops`: ArgoCD Applications, Helm values, Gateway API resources, and cluster configuration
- `GHCR`: container image registry with immutable image references by digest
- `Kind Kubernetes`: local validation environment with ArgoCD, NGINX Gateway Fabric, Kyverno, Prometheus, and Grafana

## Architecture Overview

```text
flowdx-platform
  ├─ contracts
  ├─ reusable workflows
  ├─ templates
  └─ policies
        |
        | consumed by
        v
flowdx-reference-api
  ├─ FastAPI
  ├─ Dockerfile
  ├─ Helm chart
  ├─ structured logs
  └─ flowdx.yaml
        |
        | CI/CD
        v
GHCR
  └─ image digest
        |
        v
flowdx-gitops
  ├─ ArgoCD Applications
  ├─ Helm values
  ├─ Gateway API
  └─ cluster config
        |
        | observed by
        v
Kind Kubernetes
  ├─ ArgoCD
  ├─ NGINX Gateway Fabric
  ├─ reference-api
  ├─ Kyverno
  ├─ Prometheus
  └─ Grafana
```

## Current Focus

- building `flowdx-platform` as the platform hub
- creating `flowdx-reference-api` with FastAPI, operational endpoints, and structured JSON logs
- publishing container images to GHCR
- evolving GitOps delivery with immutable image references by digest
- validating delivery on Kind with ArgoCD
- adding Gateway API routing with NGINX Gateway Fabric
- enforcing Kubernetes standards with Kyverno
- adding observability with Prometheus and Grafana

## Tech Stack

**Platform Engineering**  
`Reusable Workflows` `Declarative Contracts` `Golden Paths` `Governance Gates`

**CI/CD & Supply Chain**  
`GitHub Actions` `Docker` `GHCR` `Image Digest` `OIDC`

**Application Runtime**  
`Python` `FastAPI` `Structured Logs` `Operational Endpoints`

**Kubernetes & GitOps**  
`Kubernetes` `Kind` `Helm` `ArgoCD` `Gateway API` `NGINX Gateway Fabric`

**Governance & Observability**  
`Kyverno` `Policy-as-Code` `Prometheus` `Grafana`

**Infrastructure as Code**  
`Terraform` `Remote State` `Reusable Modules`

## Repositories

| Repository | Purpose |
| :--- | :--- |
| `flowdx-platform` | Platform hub with contracts, reusable workflows, templates, and policies. |
| `flowdx-reference-api` | FastAPI reference workload used to validate the platform delivery flow. |
| `flowdx-gitops` | GitOps source of truth for ArgoCD, Helm values, Gateway API, and cluster configuration. |

## Contact

- **LinkedIn:** [linkedin.com/in/leandro-barbosa](https://linkedin.com/in/leandrodeobarbosa)
- **Portfolio:** [leandrodeobarbosa.dev](https://leandrodeobarbosa.dev)

---

<div align="center">
  <sub>Maintained by Leandro de Oliveira Barbosa · DevOpsfera Labs · Platform Engineering Lab · 2026</sub>
</div>
