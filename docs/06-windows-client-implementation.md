# 6. Windows 11 Enterprise Client Implementation

This chapter documents the preparation of the Windows 11 Enterprise client and its connection to the `Yasha.local` domain.

## 6.1 Windows 11 Enterprise

The client virtual machine uses Windows 11 Enterprise. Microsoft provides an evaluation version through the [Windows 11 Enterprise Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise) page.

The client provides the Windows environment on which domain authentication and Group Policy behavior can be tested.

## 6.2 Creating the Client Virtual Machine

The Windows 11 Enterprise virtual machine was created in VMware Workstation Pro alongside the Windows Server 2022 virtual machine.

The two virtual machines form the basic laboratory relationship:

```text
VMware Workstation Pro
       │
       ├── Windows Server 2022
       │      └── Domain Controller / DNS
       │
       └── Windows 11 Enterprise
              └── Domain Client
```

> **Screenshot placeholder:** VMware Workstation Pro showing both virtual machines.

## 6.3 Initial Windows Setup

Windows 11 Enterprise was installed and prepared as the client operating system. To speed up the initial setup and create a local account, the following command was used during Windows setup:

```text
start ms-cxh:localonly
```

This provided access to the local-account setup path before the computer was connected to the Active Directory domain.

The initial local account should have administrative capability so that the client can be configured and joined to the domain.

> **Screenshot placeholder:** Windows 11 initial setup or local account configuration.

## 6.4 Configuring Client DNS

Before joining the domain, the Windows 11 client was configured to use the Windows Server's static IP address as its preferred DNS server.

```text
Preferred DNS Server: [WINDOWS SERVER IP - PLACEHOLDER]
```

This is a critical Active Directory requirement because domain clients use DNS to locate the Domain Controller and related domain services.

The intended communication path is:

```text
Windows 11 Enterprise
        │
        │ DNS query
        ▼
Windows Server 2022
        │
        ├── DNS
        └── AD DS
        │
        ▼
     Yasha.local
```

Using the Domain Controller's DNS service allows the client to resolve the internal `Yasha.local` domain correctly.

> **Screenshot placeholder:** Windows 11 IPv4 properties showing the Windows Server IP as Preferred DNS.

## 6.5 Testing Basic Connectivity

After configuring DNS and network settings, basic communication with the Windows Server was tested.

A basic IP connectivity test can be performed using:

```cmd
ping [SERVER-IP]
```

A successful response confirms that the client can reach the server at the IP layer. Additional DNS testing is still required because successful ping does not prove that the domain name can be resolved.

> **Screenshot placeholder:** Successful ping from Windows 11 to the Windows Server.

## 6.6 Testing DNS with `nslookup`

DNS resolution was checked using:

```cmd
nslookup Yasha.local
```

`nslookup` queries DNS and reports the server used for the lookup and the result returned for the requested name.

This test is useful because Active Directory domain operations depend on correct DNS resolution.

The distinction between the two basic tests is:

| Command | Main purpose |
|---|---|
| `ping [SERVER-IP]` | Tests IP-level network reachability. |
| `nslookup Yasha.local` | Tests DNS name resolution. |

> **Screenshot placeholder:** Successful `nslookup Yasha.local` output.

## 6.7 Renaming the Client Computer

The Windows 11 client was renamed before or during the domain-joining process so that the computer could be identified easily in the laboratory environment.

A meaningful naming convention is useful when multiple computers exist because the same computer name appears in local Windows administration and in Active Directory.

The final client computer name should be recorded here:

```text
Computer Name: [PLACEHOLDER]
```

## 6.8 Joining the Client to the Domain

After network and DNS configuration was verified, the Windows 11 client was joined to the `Yasha.local` domain.

The Windows interface used for the domain configuration was:

```text
Settings
   ↓
System
   ↓
About
   ↓
Domain or workgroup
   ↓
Domain
   ↓
Yasha.local
```

When prompted, domain administrator credentials were provided to authorize the domain join.

The domain-join process establishes the relationship between the Windows 11 computer and the Active Directory environment.

```text
Windows 11 Client
       │
       │ Domain Join Request
       ▼
DNS / Domain Controller
       │
       ▼
Yasha.local
       │
       ▼
Computer Object Created
       │
       ▼
Client becomes domain member
```

> **Screenshot placeholder:** Windows 11 domain membership dialog showing `Yasha.local`.

## 6.9 Restarting After Domain Join

After the domain join was completed, the client was restarted. A restart is necessary so Windows can fully transition into its domain-member configuration and make domain authentication available from the sign-in process.

After restart, the client should allow domain credentials to be used for authentication.

## 6.10 Verifying the Domain Membership

The client should be checked after restart to confirm that it is a member of `Yasha.local`.

Verification can include:

- Checking Windows system information for the domain membership.
- Confirming that the computer appears in Active Directory.
- Testing domain authentication with an appropriate domain account.
- Running DNS and connectivity checks again if required.

> **Screenshot placeholder:** Windows System/About page or Active Directory showing the domain-joined client.

## 6.11 Moving the Computer Object to the Correct OU

After the client joins the domain, its computer object should be located in the appropriate `Computers` OU under `Comlab1` or `Comlab2`, according to the laboratory assignment.

The intended structure is:

```text
Yasha.local
│
├── Comlab1
│   └── Computers
│       └── [CLIENT COMPUTER]
│
└── Comlab2
    └── Computers
        └── [CLIENT COMPUTER]
```

This placement is important for GPO scope when policies are linked to the corresponding OU.

## 6.12 Client Implementation Checklist

- [ ] Windows 11 Enterprise installed.
- [ ] Local administrative account configured.
- [ ] Client DNS points to the Windows Server.
- [ ] Server connectivity tested.
- [ ] `nslookup Yasha.local` tested successfully.
- [ ] Client computer renamed.
- [ ] Client joined to `Yasha.local`.
- [ ] Client restarted after domain join.
- [ ] Computer object verified in Active Directory.
- [ ] Computer object placed in the appropriate `Computers` OU.

The client is now prepared for centralized management through Group Policy.
