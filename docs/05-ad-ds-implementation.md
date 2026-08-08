# 5. Active Directory Domain Services Implementation

This chapter documents the transition from a basic Windows Server installation to a functioning Active Directory domain environment. The server is promoted to a Domain Controller and the laboratory's Active Directory structure is created.

## 5.1 Installing the AD DS Role

The first Active Directory step is installing the **Active Directory Domain Services** role on Windows Server 2022.

AD DS provides the directory service required to centrally manage users, computers, and other domain objects.

The installation is performed through Server Manager:

```text
Server Manager
    ↓
Add Roles and Features
    ↓
Active Directory Domain Services
    ↓
Install
```

Installing the role prepares the server for domain-controller promotion. At this point, the server has the AD DS role installed but is not yet the Domain Controller for `Yasha.local`.

> **Screenshot placeholder:** Server Manager showing AD DS selected/installed.

## 5.2 Promoting the Server to a Domain Controller

After installing AD DS, the server was promoted to become a Domain Controller.

Promotion changes the server from a normal Windows Server installation into a server that provides Active Directory domain services.

The project creates a new domain:

```text
Yasha.local
```

The simplified process is:

```text
Windows Server 2022
        │
        ▼
AD DS Role Installed
        │
        ▼
Domain Controller Promotion
        │
        ▼
New Forest / Domain
        │
        ▼
Yasha.local
```

During the promotion process, the required domain and directory configuration is entered, and the server is restarted after the operation is completed.

> **Screenshot placeholder:** Domain Controller promotion configuration showing `Yasha.local`.

## 5.3 The `Yasha.local` Domain

The new Active Directory domain is:

```text
Yasha.local
```

The domain acts as the central administrative namespace for the laboratory. Domain-joined Windows clients use this domain for authentication and centralized management.

The Domain Controller provides the central point through which the client discovers and communicates with Active Directory services.

## 5.4 Active Directory Users and Computers

After the server has been promoted, Active Directory Users and Computers can be used to manage the directory objects.

The project uses this tool to organize the laboratory into two main OUs:

- `Comlab1`
- `Comlab2`

Each OU contains:

- `Computers`
- `Users`

The resulting structure is:

```text
Yasha.local
│
├── Comlab1
│   ├── Computers
│   └── Users
│
└── Comlab2
    ├── Computers
    └── Users
```

> **Screenshot placeholder:** Active Directory Users and Computers showing the complete OU hierarchy.

## 5.5 Creating the Organizational Units

The OUs were created to provide a logical separation between the two computer laboratories.

The structure was created as follows:

1. Create `Comlab1` under the domain.
2. Create `Computers` inside `Comlab1`.
3. Create `Users` inside `Comlab1`.
4. Create `Comlab2` under the domain.
5. Create `Computers` inside `Comlab2`.
6. Create `Users` inside `Comlab2`.

This structure is important for later Group Policy management because GPOs can be linked to appropriate OUs.

## 5.6 Creating User Accounts

User accounts were created for the laboratory computers and organized within the appropriate `Users` OU.

A domain user account provides a centralized identity that can be used to authenticate on a domain-joined Windows computer.

When documenting the actual accounts, use the names created in the laboratory. Do not publish passwords or other authentication secrets in the repository.

> **Screenshot placeholder:** Active Directory Users and Computers showing the created user accounts.

## 5.7 Computer Objects

When a Windows client joins the domain, a corresponding computer object is created in Active Directory. The computer object represents the client within the domain and can be moved into the intended `Computers` OU.

This creates the relationship:

```text
Windows 11 Client
       │
       │ Domain Join
       ▼
Active Directory
       │
       ▼
Computer Object
       │
       ▼
Comlab1 or Comlab2
       │
       ▼
Computers OU
```

The exact client computer name will be recorded as a placeholder until the laboratory screenshot and configuration details are added.

## 5.8 Why the OU Structure Matters for GPO

The OU structure is more than a way to make Active Directory look organized. It provides a management boundary for policies and objects.

For example, a policy intended only for `Comlab1` can be linked to that OU rather than being linked to the entire domain. Similarly, computer-focused policies can be targeted at a `Computers` OU.

This makes the design easier to expand as the laboratory grows.

## 5.9 AD DS Verification

Before moving to the Windows client configuration, the server-side Active Directory environment should be checked.

The verification should confirm:

- The `Yasha.local` domain exists.
- The server is functioning as a Domain Controller.
- `Comlab1` and `Comlab2` exist.
- Each laboratory contains `Computers` and `Users` OUs.
- The intended user accounts have been created.
- DNS is available for the domain environment.

> **Screenshot placeholder:** Active Directory and DNS evidence confirming the domain is operational.

## 5.10 AD DS Implementation Checklist

- [ ] AD DS role installed.
- [ ] Server promoted to Domain Controller.
- [ ] `Yasha.local` created successfully.
- [ ] `Comlab1` created.
- [ ] `Comlab2` created.
- [ ] `Computers` and `Users` OUs created under both laboratories.
- [ ] Required user accounts created.
- [ ] Domain Controller and DNS services verified.

The next chapter moves to the Windows 11 Enterprise client and explains how the client was prepared, configured to use the Domain Controller for DNS, and joined to `Yasha.local`.
