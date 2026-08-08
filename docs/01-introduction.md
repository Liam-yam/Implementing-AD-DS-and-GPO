# 1. Introduction and Project Scope

## 1.1 Project Overview

This project documents the design, implementation, and testing of a small Windows Server laboratory environment using VMware Workstation Pro, Windows Server 2022, and Windows 11 Enterprise. The laboratory was created to demonstrate how a Windows Server can provide centralized identity, computer management, DNS, and Group Policy services for a computer laboratory.

The environment uses one Windows Server 2022 virtual machine as the Domain Controller and one Windows 11 Enterprise virtual machine as the client. The server hosts Active Directory Domain Services (AD DS) and DNS, while the Windows 11 client is joined to the `Yasha.local` domain and receives centrally managed Group Policy settings.

The project is presented both as a technical report and as a practical laboratory guide. The report explains the concepts behind the implementation, while the laboratory sections document the configuration sequence and the reasoning behind important decisions.

## 1.2 Project Goal

The main goal of the project is to build a functional Windows Server laboratory environment and implement Group Policy for centralized management of Windows client computers.

The project specifically aims to:

- Deploy Windows Server 2022 and Windows 11 Enterprise as virtual machines using VMware Workstation Pro.
- Configure Windows Server as a Domain Controller for the `Yasha.local` Active Directory domain.
- Organize laboratory users and computers using the `Comlab1` and `Comlab2` Organizational Units (OUs).
- Join the Windows 11 client to the domain and verify communication through DNS and network testing.
- Implement Group Policy settings that address common configuration problems in shared computer laboratories.

## 1.3 Project Scope

The implementation focuses on the Windows Server technologies that are most relevant to a shared computer laboratory. It does not attempt to cover every Windows Server role or enterprise feature.

The scope includes:

- Windows Server 2022 installation and basic server configuration.
- Static IP addressing and DNS configuration for the server and client environment.
- Active Directory Domain Services installation and Domain Controller promotion.
- Creation of the `Yasha.local` domain, Organizational Units, users, and computers.
- Windows 11 Enterprise client configuration, domain joining, and basic connectivity verification.
- Group Policy implementation for desktop wallpaper, password management, and selected Control Panel restrictions.

Optional features such as website or URL blocking are outside the current implemented scope and may be considered as future improvements.

## 1.4 Intended Audience

This documentation is intended for students learning Windows Server administration, IT practitioners who want a concise laboratory reference, and IT instructors or professors reviewing the implementation.

The documentation therefore combines technical explanations with practical steps. Concepts are introduced before the related configuration so that the reader understands not only what was configured, but also why it was necessary.

## 1.5 Laboratory Approach

The project followed a progressive implementation approach rather than configuring all services at once. The server environment was established first, followed by Active Directory, the client machine, domain membership, and finally Group Policy.

The overall sequence was:

```text
VMware Workstation Pro
        │
        ▼
Windows Server 2022
        │
        ├── Initial configuration
        ├── Static IP
        ├── AD DS installation
        └── Domain Controller promotion
                │
                ▼
           Yasha.local
                │
        ┌───────┴───────┐
        ▼               ▼
    Comlab1          Comlab2
        │               │
   Users/Computers  Users/Computers
        │
        └───────┬───────┘
                ▼
      Windows 11 Enterprise
                │
        ├── DNS configuration
        ├── Connectivity testing
        └── Domain joining
                │
                ▼
            Group Policy
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    Desktop  Password  Control Panel
     Policy   Policy      Policy
```

This sequence also provides the structure for the remaining chapters of the documentation.

## 1.6 Learning Journey

The Windows Server, AD DS, DNS, and GPO concepts were studied and implemented over approximately one to two weeks. The learning approach was intentionally focused on the features that are useful in a computer laboratory rather than attempting to study the entire Windows Server platform.

The main learning areas were:

- Understanding the role of Windows Server and how server roles support client computers.
- Understanding AD DS, Domain Controllers, domains, Organizational Units, users, and computers.
- Understanding why DNS is essential to reliable Active Directory operation.
- Learning how Group Policy provides centralized configuration and helps prevent unwanted changes on shared computers.

## 1.7 Documentation and Evidence

Screenshots of the actual laboratory configuration will be added after the written implementation has been completed. Screenshot placeholders will identify where evidence should be inserted.

Each implementation section will follow a consistent pattern:

1. **Purpose** — explains why the configuration is required.
2. **Procedure** — documents the steps performed in the laboratory.
3. **Configuration** — records the important values or settings used.
4. **Verification** — explains how the result was tested.

This approach is intended to make the documentation reproducible while clearly separating actual laboratory evidence from general technical explanation.

> **Screenshot placeholder:** Add a project overview or VMware laboratory screenshot here when available.

## 1.8 Project Limitations

The laboratory is intentionally small and represents a learning environment rather than a production enterprise network. It uses one Windows Server 2022 Domain Controller and one Windows 11 Enterprise client, so it does not demonstrate redundancy or multiple-domain-controller scenarios.

The environment also uses placeholder network values in the documentation until the exact laboratory addressing information is added. Future improvements may include additional clients, more detailed security policies, backup and recovery procedures, and website restrictions.
