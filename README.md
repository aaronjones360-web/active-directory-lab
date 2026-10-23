# Active Directory Domain Services (AD DS) Enterprise Architecture & Automation Lab

## Overview
This project documents the design, deployment, configuration, and automated administration of an isolated Windows Server Active Directory Domain Services (AD DS) infrastructure (`adlab.test`) hosted on Microsoft Hyper-V. 

The objective is to demonstrate enterprise-grade Windows Server administration, multi-tiered Organizational Unit (OU) design, Role-Based Access Control (RBAC), Group Policy security baselines, and Infrastructure-as-Code (IaC) using PowerShell.

---

## Core Objectives & Architecture
* **Domain Infrastructure:** Deployed `adlab.test` on Windows Server with integrated DNS and DHCP services.
* **Tiered OU Hierarchy:** Architected an `Apex` root OU structure separating Tier-1 Administrators, Workstations, Service Accounts, and Business Departments (HR, IT, Finance, Executive).
* **Access Control Strategy:** Enforced **AGDLP** (Account -> Global Group -> Domain Local Group -> Permission) security baselines across departmental resource groups.
* **Least-Privilege Delegation:** Delegated granular OU-level administrative rights (password resets, computer object joins) without exposing Domain Admin privileges.
* **Security Baselines:** Applied GPO baselines enforcing workstation idle lockouts, account lockout policies, and local security auditing.
* **PowerShell Automation:** Authored idempotent PowerShell scripts (`New-ApexInfrastructure.ps1`) to programmatically build, verify, and audit user populations and OU structures.

---

## Repository Structure
```text
active-directory-lab/
│
├── README.md           <-- Project documentation & architecture overview
├── scripts/            <-- PowerShell deployment & auditing scripts
│   └── New-ApexInfrastructure.ps1
└── images/             <-- Verification screenshots (OU tree, GPO baselines)
    ├── ou-structure.png
    └── gpo-baselines.png
