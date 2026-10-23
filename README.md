# Enterprise Active Directory (AD DS) Infrastructure & Automated Migration Lab

## Executive Summary
This project documents the end-to-end design, deployment, configuration, and automated remediation of a Windows Server Active Directory Domain Services (`adlab.test`) environment hosted on Microsoft Hyper-V. 

The lab demonstrates foundational systems engineering, core directory services setup, Group Policy enforcement, and PowerShell Infrastructure-as-Code (IaC) bulk user migration.

---

## Phase 1: Virtualization & Infrastructure Base (Hyper-V Setup)
* **Hyper-V Hypervisor Configuration:** Enabled and configured Microsoft Hyper-V role on host hardware.
* **Virtual Network Architecture:** Configured an isolated Internal Virtual Switch (`ADLab-PrivateNet`) to secure VM-to-VM traffic and manage internal routing.
* **Virtual Machine Provisioning:** Created host virtual machines with allocated vCPUs, RAM baselines, and dynamic VHDX storage for Windows Server.

> **Verification Artifacts (Phase 1):**
> * `images/phase1-hyperv-virtual-switch.png` — Hyper-V Virtual Switch Manager showing network isolation setup.
> * `images/phase1-vm-configuration.png` — Virtual Machine hardware settings and running state.

---

## Phase 2: Active Directory Domain Services (AD DS) Deployment & Verification
* **Identity Role Installation:** Installed AD DS, DNS, and DHCP server roles on Windows Server.
* **Forest & Domain Promotion:** Promoted server to Root Domain Controller for `adlab.test`.
* **Core Verification Steps:**
  * Confirmed Domain Controller promotion and DNS resolution via `Get-ADDomain` and `Resolve-DnsName adlab.test`.
  * Verified Active Directory Administrative Center accessibility and core system containers.

> **Verification Artifacts (Phase 2):**
> * `images/phase2-adds-role-installation.png` — Server Manager showing AD DS and DNS roles installed.
> * `images/phase2-domain-promotion-confirmation.png` — PowerShell output verifying `adlab.test` forest status.

---

## Phase 3: Directory Architecture, Initial Objects & Group Policy Baselines
* **Initial OU Hierarchy:** Interactively established baseline OUs and security groups.
* **Group Policy Object (GPO) Deployment:** Applied enterprise security baselines:
  * Workstation idle lockout (15-minute screen saver lockout policy).
  * Account lockout thresholds and audit logging policies.
* **Initial User & Group Provisioning:** Populated core administration accounts, default user containers, and departmental groups.

> **Verification Artifacts (Phase 3):**
> * `images/phase3-initial-ou-structure.png` — Active Directory Users & Computers showing initial container layout.
> * `images/phase3-gpo-baselines.png` — Group Policy Management Console displaying linked security GPOs.

---

## Phase 4: Automated Remediation & Bulk Migration to `Apex` OU (`ApexInfrastructure.ps1`)
* **Operational Challenge:** Address 150 unorganized domain users and legacy OUs residing in default/misplaced containers outside the target security perimeter.
* **PowerShell Remediation Workflow:** Authored and executed `ApexInfrastructure.ps1` to:
  1. Build out the structured `Apex` root and sub-OU hierarchy (`Administration`, `Workstations`, `ServiceAccounts`, `Departments`).
  2. Create AGDLP security groups (`GG_IT_Staff`, `GG_HR_Staff`, etc.).
  3. Programmatically parse, extract, and migrate all 150 misplaced users into their respective `Apex` departmental OUs.
  4. Assign users to respective AGDLP Global and Domain Local access control groups.

> **Verification Artifacts (Phase 4):**
> * `images/phase4-misplaced-users-before.png` — ADUC showing misplaced user accounts in unorganized containers.
> * `images/phase4-script-execution.png` — PowerShell terminal output executing `ApexInfrastructure.ps1`.
> * `images/phase4-remediated-apex-structure.png` — Clean `Apex` OU tree with verified user placement and group memberships.

---

## Repository Structure
```text
active-directory-lab/
│
├── README.md                           <-- Project overview & phase walkthrough
├── scripts/
│   └── ApexInfrastructure.ps1          <-- Phase 4 migration & remediation script
└── images/
    ├── phase1-hyperv-virtual-switch.png
    ├── phase1-vm-configuration.png
    ├── phase2-adds-role-installation.png
    ├── phase2-domain-promotion-confirmation.png
    ├── phase3-initial-ou-structure.png
    ├── phase3-gpo-baselines.png
    ├── phase4-misplaced-users-before.png
    ├── phase4-script-execution.png
    └── phase4-remediated-apex-structure.png
