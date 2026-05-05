# 🛠️ DevOpsfera Labs

<div align="center">
  <img src="https://img.shields.io/badge/Maintained%20by-Leandro%20Barbosa-blue?style=flat-square&logo=github" alt="Maintained by Leandro Barbosa">
  <img src="https://img.shields.io/badge/Focus-Platform%20Engineering-black?style=flat-square" alt="Focus: Platform Engineering">
  <img src="https://img.shields.io/badge/Core%20Project-FlowDX-orange?style=flat-square" alt="Core Project: FlowDX">
</div>

## About

**DevOpsfera Labs** is the engineering workspace behind **FlowDX**, a Platform Engineering initiative focused on self-service workload delivery, reusable automation, GitOps, governance, and developer experience(DevEx).

The goal of this organization is to demonstrate production-oriented DevOps and Platform Engineering patterns through a practical reference architecture: a platform repository providing reusable workflows, contracts, templates, and policies; a reference application consuming those platform capabilities; and a GitOps repository responsible for declarative Kubernetes delivery.

FlowDX is not positioned as a full Internal Developer Platform at this stage. It is a focused platform capability for standardizing how workloads are validated, built, delivered, and governed.

## Core Project: FlowDX

**FlowDX** is a self-service delivery platform built around reusable workflows, declarative contracts, GitOps, policy-as-code, and governed automation.

Instead of allowing workloads to define arbitrary delivery logic from scratch, FlowDX provides a controlled path where application repositories consume standardized platform capabilities.

The intended model is:

~~~text
flowdx-platform
    |
    | provides contracts, reusable workflows, templates, and policies
    v
flowdx-reference-api
    |
    | builds and publishes container image through CI/CD
    v
Container Registry
    |
    | image consumed by GitOps manifests
    v
flowdx-gitops
    |
    | observed by ArgoCD
    v
Kind Kubernetes
~~~

## Repository Model

The FlowDX architecture is organized around three main repository responsibilities.

### `flowdx-platform`

Central platform repository.

Owns the reusable platform capabilities consumed by workloads:

- declarative contracts
- reusable GitHub Actions workflows
- workflow templates
- policy-as-code rules
- Kyverno policies
- validation standards
- delivery conventions
- governance patterns

This repository represents the platform layer. It should not contain application-specific business logic.

### `flowdx-reference-api`

Reference workload repository.

Represents a realistic application team repository consuming FlowDX platform capabilities.

Expected contents:

- Python API
- Dockerfile
- Helm chart
- `flowdx.yaml`
- GitHub Actions workflow consuming reusable workflows from `flowdx-platform`

This repository is used to validate the developer experience of the platform: how a service is built, checked, containerized, and prepared for GitOps delivery.

### `flowdx-gitops`

GitOps repository.

Owns the desired state consumed by ArgoCD.

Expected contents:

- ArgoCD Applications
- Helm values
- environment-specific configuration
- cluster bootstrap manifests
- workload deployment references
- image tag updates produced by the delivery flow

This repository represents the deployment source of truth for Kubernetes workloads.

## Reference Delivery Flow

The current reference flow is based on a Python API workload delivered to a local Kubernetes cluster using GitOps.

~~~text
flowdx-platform
  ├─ contracts
  ├─ reusable workflows
  ├─ templates
  └─ kyverno policies
        |
        | consumed by
        v
flowdx-reference-api
  ├─ Python API
  ├─ Dockerfile
  ├─ Helm chart
  └─ flowdx.yaml
        |
        | CI/CD
        v
Container Registry
  └─ GHCR
        |
        | image
        v
flowdx-gitops
  ├─ ArgoCD Applications
  ├─ Helm values
  └─ cluster config
        |
        | observed by
        v
Kind Kubernetes
  ├─ ArgoCD
  ├─ reference-api
  ├─ Prometheus
  ├─ Grafana
  └─ Kyverno
~~~

## Platform Capabilities

FlowDX is designed to explore a controlled self-service model where developers consume paved roads instead of rebuilding delivery logic for every workload.

Planned capabilities include:

- contract-based workload configuration using `flowdx.yaml`
- reusable GitHub Actions workflows
- standardized Docker build and publish flow
- GHCR-based image publishing
- Helm-based workload packaging
- GitOps delivery through ArgoCD
- image tag updates in the GitOps repository
- Kubernetes policy enforcement with Kyverno
- local cluster validation with Kind
- observability baseline with Prometheus and Grafana
- governed pull request flow
- required status checks
- sanitized pipeline summaries
- environment-specific approval gates

## `flowdx.yaml`

The `flowdx.yaml` file acts as the workload contract between an application repository and the FlowDX platform.

A minimal example:

~~~yaml
apiVersion: flowdx.dev/v1alpha1
kind: Workload

metadata:
  name: reference-api
  owner: platform-lab
  costCenter: devopsfera-labs

spec:
  runtime: python
  delivery:
    type: gitops
    registry: ghcr
    chartPath: charts/reference-api

  environments:
    - name: dev
      cluster: kind
      namespace: reference-api
      autoSync: true

  governance:
    requireApproval: false
    policyProfile: baseline
~~~

The goal of this contract is not to expose raw infrastructure primitives. It should describe the workload intent while the platform owns the implementation details.

## Governance Model

FlowDX follows a governed automation model.

The platform should approve standard paths automatically and require human review only for exceptions.

Examples of automated validations:

- contract schema validation
- required metadata checks
- naming convention checks
- allowed environment rules
- Dockerfile validation
- image build validation
- Helm chart validation
- Kubernetes manifest validation
- Kyverno policy checks
- security scanning
- GitOps diff checks
- required status checks before merge

The preferred model is:

~~~text
standard request
  -> automated validation
  -> merge allowed
  -> delivery proceeds

exception request
  -> policy violation or sensitive change detected
  -> explicit review required
  -> merge blocked until approved
~~~

This avoids turning GitHub into a ticketing system while still preserving control, traceability, and operational safety.

## Security and Auditability

FlowDX should favor secure automation over broad exposure of infrastructure details.

Guidelines:

- use GitHub OIDC instead of long-lived cloud credentials
- apply least privilege to CI/CD roles
- avoid exposing raw infrastructure plans in public PR comments
- prefer sanitized summaries for pull request feedback
- treat plans, logs, artifacts, and state-derived outputs as potentially sensitive
- use short retention for sensitive artifacts when artifacts are required
- publish only the outputs required by the consuming workload
- keep environment-specific approvals for sensitive operations

The platform should provide enough feedback for developers to understand what happened without leaking unnecessary infrastructure metadata.

## Engineering Roadmap

| Phase | Goal | Status |
| :--- | :--- | :--- |
| **01: Platform Foundation** | Create `flowdx-platform` with contracts, reusable workflows, templates, and initial governance standards. | 🚧 In Progress |
| **02: Reference API Workload** | Create `flowdx-reference-api` as the first workload consuming platform workflows. | 🗓️ Planned |
| **03: Container Delivery** | Build and publish the Python API image to GHCR through standardized CI/CD. | 🗓️ Planned |
| **04: GitOps Repository** | Create `flowdx-gitops` with ArgoCD Applications, Helm values, and cluster configuration. | 🗓️ Planned |
| **05: Local Kubernetes Validation** | Validate the full flow on Kind with ArgoCD, reference-api, Prometheus, Grafana, and Kyverno. | 🗓️ Planned |
| **06: Governance & Security** | Add policy-as-code, required checks, approval gates, sanitized summaries, and security scanning. | 🗓️ Planned |
| **07: Platform Maturity** | Improve contract versioning, workflow versioning, release strategy, documentation, and operational runbooks. | 🚀 Future |

## Tech Stack

**Platform Engineering**  
`FlowDX` `Reusable Workflows` `Declarative Contracts` `Golden Paths` `Governance Gates`

**CI/CD & Automation**  
`GitHub Actions` `Reusable Workflows` `OIDC` `GHCR` `Docker`

**Application Runtime**  
`Python` `Container Images` `Helm`

**GitOps & Kubernetes**  
`ArgoCD` `Kind` `Kubernetes` `Helm Values` `Declarative Delivery`

**Governance & Security**  
`Kyverno` `Policy-as-Code` `Required Checks` `Approval Gates` `Least Privilege`

**Observability**  
`Prometheus` `Grafana`

**Infrastructure as Code**  
`Terraform` `Remote State` `Reusable Modules`

## Why this exists

FlowDX exists to explore how platform teams can provide autonomy to developers without losing control over security, delivery standards, operational consistency, and governance.

The current focus is not to build a complete IDP.

The focus is to build a practical and credible platform foundation where:

- workloads consume reusable delivery workflows
- application configuration is expressed through contracts
- delivery is standardized
- GitOps owns runtime state
- policies enforce platform rules
- feedback is automated and safe to expose
- local Kubernetes validates the platform flow before expanding to cloud environments

## Project Scope

Current scope:

- platform repository
- reference Python API workload
- GitHub Actions reusable workflows
- GHCR image publishing
- GitOps repository
- ArgoCD-based delivery
- Kind-based Kubernetes validation
- Kyverno policy enforcement
- Prometheus and Grafana baseline

Out of scope for the current stage:

- full Internal Developer Platform
- Backstage integration
- multi-cloud abstraction
- production-grade multi-tenant Kubernetes
- complex service catalog
- CLI-first developer experience

These may be explored later, but they are not required for the first credible version of FlowDX.

## Contact

- **LinkedIn:** [linkedin.com/in/leandro-barbosa](https://linkedin.com/in/leandro-barbosa)
- **Portfolio:** [leandrodeobarbosa.dev](https://leandrodeobarbosa.dev)

---

<div align="center">
  <sub>Maintained by Leandro de Oliveira Barbosa · DevOpsfera Labs · Platform Engineering Lab · 2026</sub>
</div>
