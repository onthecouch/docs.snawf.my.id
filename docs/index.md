---
hide:
  - navigation
  - toc
---

# System Documentation


---

## 1. Overview

This sites serves as the authoritative technical reference for the infrastructure. It documents system setups, configuration decisions, and operational procedures.

*   **Objective:** To ensure long-term recall and operational continuity.

*   **Philosophy:** Function over form. The content assumes baseline knowledge and focuses on implementation details, specific configurations, and architectural reasoning.

*   **Source:** All documentation is derived from active deployments, real-world troubleshooting, and validated configurations.

---

## 2. Documentation Scope

Primary areas of coverage within the documentation site.

| Category | Description | Key Technologies |
| :--- | :--- | :--- |
| **Homelab** | On-premise infrastructure and virtualization. | Proxmox, LXC, Networking, DNS |
| **Windows** | Workstation environment and tooling. | PowerShell, Winget, Driver Fixes |
| **Oracle Cloud** | External cloud resources and hybrid connectivity. | Oracle Free Tier, Ubuntu ARM, Tailscale |

---

## 3. Domain Specifics

### 3.1 Homelab Environment
Documentation regarding the Proxmox-based private cloud. Content focuses on practical configuration and stability trade-offs rather than theoretical implementation.

*   **Host Management:** Bootstrapping and clustering.

*   **Virtualization:** VM and Container (LXC) provisioning.

*   **Networking:** VLAN segmentation, internal DNS strategies, and tunneling.

*   **Services:** Reverse proxy configuration and service hardening.

### 3.2 Windows Workstation
Operational notes for the daily driver environment, detailing solutions for hardware idiosyncrasies and workflow optimization.

*   **Automation:** PowerShell profile customization and script management.

*   **Tooling:** Development environment setup and quality-of-life adjustments.

*   **Troubleshooting:** Resolved driver issues and hardware-specific fixes.

### 3.3 Oracle Cloud
Architectural notes for cloud-hosted workloads, emphasizing cost-effectiveness and reliability.

*   **Infrastructure:** Lightweight deployments on Oracle Free Tier (ARM64).

*   **Connectivity:** Secure bridging to on-premise systems via VPN.

*   **Architecture:** Simple, robust configuration patterns for hybrid operations.
