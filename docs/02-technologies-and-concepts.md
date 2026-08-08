# 2. Technologies and Core Concepts

This section introduces the technologies used in the laboratory before documenting their implementation. Understanding the role of each component makes the later configuration steps easier to follow and provides the technical context for the design decisions made in the project.

## 2.1 VMware Workstation Pro

VMware Workstation Pro is a desktop virtualization platform that allows multiple operating systems to run as virtual machines on one physical computer. In this project, it provides the isolated laboratory environment used to run Windows Server 2022 and Windows 11 Enterprise.

Virtualization is useful for this project because:

- A server and client environment can be built without requiring separate physical computers.
- Virtual machines can be configured, tested, reset, and expanded independently.
- The laboratory can be reproduced on compatible hardware with the required virtual machine resources.
- Snapshots or backups can provide a convenient recovery point during experimentation.

**Official installation resource:** [VMware Workstation Pro installation resource](https://knowledge.broadcom.com/external/article?articleNumber=368667)

## 2.2 Windows Server 2022

Windows Server is Microsoft's server operating system for providing centralized network, identity, management, and application services. In this project, Windows Server 2022 provides the central services required by the laboratory, primarily Active Directory Domain Services, DNS, and Group Policy management.

The Windows Server evaluation used for the project is available from Microsoft's Evaluation Center: [Windows Server 2022 Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022).

The server's main responsibilities in this laboratory are:

- Acting as the Domain Controller for the `Yasha.local` domain.
- Hosting Active Directory Domain Services for centralized user and computer management.
- Providing DNS services needed for Active Directory name resolution and service discovery.
- Providing the management platform from which Group Policy settings are configured and applied to domain clients.

## 2.3 Active Directory

Active Directory is Microsoft's directory service for organizing and managing network identities and resources. It stores information about objects such as users, computers, groups, and Organizational Units and allows administrators to manage those objects centrally.

In a computer laboratory, centralized directory management is valuable because:

- User accounts can be managed from a central location.
- Computers can be organized logically instead of being managed as unrelated machines.
- Authentication can be handled by the domain rather than by separate local accounts on every computer.
- Policies can be applied consistently to multiple computers and users.

The simplified relationship is:

```text
                    Active Directory
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
        Users           Computers         Groups
          │                │
          └────────────────┴────────────────┘
                           │
                    Centralized Management
```

## 2.4 Active Directory Domain Services (AD DS)

Active Directory Domain Services (AD DS) is the Windows Server role that provides the directory and domain services used by an Active Directory environment. AD DS stores directory objects and provides the authentication and management framework for domain-joined Windows computers.

The important AD DS concepts used in this project are:

- **Domain:** The administrative boundary containing the users, computers, and other directory objects. The project domain is `Yasha.local`.
- **Domain Controller:** The Windows Server that hosts AD DS and handles domain authentication and directory services.
- **User object:** Represents a user account managed by the domain.
- **Computer object:** Represents a domain-joined computer in Active Directory.
- **Organizational Unit:** A logical container used to organize objects and provide a scope for management and Group Policy.

## 2.5 Domain Controller

A Domain Controller (DC) is a server that has been promoted to provide Active Directory Domain Services for a domain. It authenticates domain users, manages directory information, and provides services required by domain members.

In this project, the Windows Server 2022 machine is promoted as the Domain Controller for `Yasha.local`.

The basic authentication relationship is:

```text
User enters domain credentials
             │
             ▼
      Windows 11 Client
             │
             ▼
       Domain Controller
             │
             ▼
        Active Directory
             │
             ▼
       Credentials checked
             │
             ▼
        Access granted
```

## 2.6 DNS and Active Directory

The Domain Name System (DNS) translates names into IP addresses and allows computers to locate network services. DNS is especially important to Active Directory because domain clients use DNS records to locate Domain Controllers and other domain services.

For this reason, the Windows 11 client in this project is configured to use the Windows Server's IP address as its preferred DNS server.

The relationship can be summarized as:

```text
Windows 11 Client
       │
       │ DNS query
       ▼
Windows Server / DNS
       │
       ▼
Yasha.local
       │
       ▼
Domain services located
```

This also explains why DNS configuration must be checked when a client cannot locate or join the domain.

## 2.7 Organizational Units (OUs)

An Organizational Unit is a logical container within Active Directory used to organize users, computers, and other objects. OUs also provide a useful scope for applying Group Policy.

The project uses two laboratory OUs:

- `Comlab1` for the first computer laboratory.
- `Comlab2` for the second computer laboratory.

Each laboratory OU contains:

- `Computers` — for computer objects assigned to that laboratory.
- `Users` — for user accounts associated with that laboratory.

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

This organization allows future policies to be targeted at one laboratory or one object type without requiring every domain object to share the same policy scope.

## 2.8 Users and Groups

Domain users are accounts stored and managed by Active Directory. In this project, user accounts were created for the laboratory computers and placed within the appropriate `Users` OU.

Centralized user management provides several benefits:

- Administrators can manage accounts from the Domain Controller.
- Domain credentials can be used to authenticate on domain-joined computers.
- User accounts can be organized according to the laboratory structure.
- Access and policy management can be standardized rather than configured separately on each client.

Groups can also be used to organize users and simplify permissions and policy targeting. Where groups are not required for a particular implementation step, the documentation will not claim that they were configured.

## 2.9 Group Policy

Group Policy is a Windows management feature that allows administrators to centrally configure settings for users and computers in an Active Directory environment.

Group Policy is particularly useful in a computer laboratory because shared computers should normally maintain a consistent configuration. Instead of manually changing each computer, an administrator can configure a policy centrally and apply it to the appropriate scope.

The project uses Group Policy to:

- Standardize the desktop wallpaper.
- Manage password-related settings.
- Restrict selected Control Panel configuration areas.
- Reduce accidental or unwanted changes by laboratory users.

## 2.10 Group Policy Object (GPO)

A Group Policy Object (GPO) is a collection of policy settings that can be configured and linked to an appropriate Active Directory scope, such as an Organizational Unit.

The basic process is:

```text
Create GPO
   │
   ▼
Configure policy settings
   │
   ▼
Link GPO to appropriate OU
   │
   ▼
Domain client processes policy
   │
   ▼
Settings are applied
```

The project implements three GPO categories:

1. **Desktop Policy** — standardizes the wallpaper and prevents users from changing it.
2. **Password Policy** — applies the password-related controls configured for the laboratory.
3. **Control Panel Policy** — restricts selected areas such as User Accounts and Network and Sharing Center.

## 2.11 Client Operating System

The client uses Windows 11 Enterprise. It provides the Windows environment that will be joined to the `Yasha.local` domain and managed through Group Policy.

Microsoft's evaluation version is available through the Evaluation Center: [Windows 11 Enterprise Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise).

The client is responsible for:

- Connecting to the laboratory network.
- Using the Windows Server as its preferred DNS server.
- Joining the `Yasha.local` domain.
- Authenticating domain users.
- Receiving and applying Group Policy settings.

## 2.12 How the Technologies Work Together

The laboratory is not a collection of unrelated technologies. Each component supports another part of the environment.

```text
                 VMware Workstation Pro
                          │
              Provides virtual machines
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
 Windows Server 2022               Windows 11 Enterprise
          │                               │
          ├── AD DS                      │
          │      │                        │
          │      └── Yasha.local ◄────────┤
          │                               │
          ├── DNS ◄───────────────────────┤
          │                               │
          └── Group Policy ───────────────►
                          │
                          ▼
                 Centralized Management
```

This relationship is the foundation of the implementation documented in the following chapters. The next sections move from these concepts into the actual laboratory configuration.

> **Screenshot placeholder:** Add a screenshot of the overall VMware laboratory or a simplified network diagram here when evidence is available.
