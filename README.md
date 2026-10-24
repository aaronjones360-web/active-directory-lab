# Enterprise Active Directory (AD DS) Infrastructure & Automated Deployment Lab

## Executive Summary
This project documents the end-to-end design, deployment, configuration, and Infrastructure-as-Code (IaC) provisioning of a Windows Server Active Directory Domain Services (`adlab.test`) environment hosted on Microsoft Hyper-V.

The lab demonstrates foundational systems engineering, core directory services setup, Group Policy enforcement, and automated bulk user/OU deployment using PowerShell.

---

## Repository Structure
```text
active-directory-lab/
│
├── README.md                           <-- Project overview & phase walkthrough
├── LICENSE                             <-- MIT License
├── scripts/
│   └── Populate-ApexInfrastructure.ps1 <-- IaC Script: Builds 5 OUs, AGDLP Groups & provisions 100 domain users
└── images/
    ├── phase1-hyperv-setup.png
    ├── phase1-rras-nat-configuration.png
    ├── phase2-adds-domain-promotion-confirmation.png
    ├── phase2-adds-role-installation.png
    ├── phase2-dhcp-scope-configuration.png
    ├── phase3-domain-login-verification.png
    ├── phase3-gpo-baselines.png
    ├── phase3-initial-ou-structure.png
    └── phase4-tiered-users.png
