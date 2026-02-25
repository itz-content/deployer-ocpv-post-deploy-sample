# deployer-ocpv-post-deploy-sample

A sample Ansible playbook that demonstrates how to leverage the **IBM Technology Zone (ITZ) Deployment Operator** to automate post-deployment configuration on OpenShift Virtualization environments.

This sample installs two workloads:

- **IBM Cloud Pak for Data** — an enterprise data and AI platform deployed via the ITZ Deployment Operator
- **Red Hat Showroom** — a lightweight lab documentation hosting solution that runs natively on OpenShift, making it easy to deliver guided lab content alongside your deployed environment

---

## Overview

The IBM Technology Zone Deployment Operator simplifies the installation of complex software stacks on OpenShift by providing a declarative, operator-driven approach to post-deployment automation. This repository serves as a reference implementation showing how to plug your own Ansible playbook into that workflow.

By combining Cloud Pak for Data with Red Hat Showroom, this sample also demonstrates a common ITZ content pattern: deploying a platform *and* the documentation that teaches users how to use it, all within a single automated workflow.

---

## Repository Structure

```
.
├── group_vars/          # Ansible group variables (configuration for each workload)
├── roles/
│   └── showroom/        # Ansible role for deploying Red Hat Showroom on OpenShift
├── site.yml             # Main playbook entrypoint
├── parameters.yml       # Deployment parameters consumed by the ITZ Deployment Operator
├── requirements.yml     # Ansible Galaxy collection dependencies
└── ansible.cfg          # Ansible configuration
```

---

## What Gets Deployed

### IBM Cloud Pak for Data
Cloud Pak for Data is IBM's unified data and AI platform. In this sample, the ITZ Deployment Operator handles the heavy lifting of installing and configuring CPD on your OpenShift cluster.

### Red Hat Showroom
[Red Hat Showroom](https://github.com/rhpds/showroom) is an OpenShift-native solution for hosting lab guides and documentation. It renders Antora-based content directly inside your cluster, giving lab participants a seamless, self-contained experience without needing to host docs externally.

This sample includes a dedicated Ansible role (`roles/showroom`) that deploys and configures Showroom as part of the post-deployment workflow.

---

## Prerequisites

- An OpenShift cluster (OCP-V / OpenShift Virtualization)
- IBM Technology Zone Deployment Operator installed on the cluster
- Ansible and the required collections (`requirements.yml`)
- Sufficient cluster resources for Cloud Pak for Data

---

## Getting Started

1. **Clone this repository**

   ```bash
   git clone https://github.com/itz-content/deployer-ocpv-post-deploy-sample.git
   cd deployer-ocpv-post-deploy-sample
   ```

2. **Install Ansible dependencies**

   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

3. **Configure your parameters**

   Edit `parameters.yml` and `group_vars/` to match your environment (cluster credentials, Cloud Pak for Data license, Showroom content source, etc.).

4. **Run the playbook**

   ```bash
   ansible-playbook site.yml
   ```

   Or, if triggered via the ITZ Deployment Operator, the operator will handle execution automatically based on the `parameters.yml` configuration.

---

## How It Relates to the ITZ Deployment Operator

The ITZ Deployment Operator is designed to consume Ansible-based post-deploy playbooks like this one. It reads the `parameters.yml` file to understand what to deploy and then drives `site.yml` against your target cluster. This sample repository is structured to be directly compatible with that contract, making it a useful starting point for building your own ITZ-compatible content.

---

## License

This project is licensed under the [Apache License 2.0](LICENSE).