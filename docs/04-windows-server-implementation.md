# 4. Windows Server 2022 Implementation

This chapter documents the initial server-side preparation of the laboratory. The Windows Server 2022 virtual machine provides the foundation for the Active Directory and Group Policy environment.

## 4.1 Deploying Windows Server 2022

The first major implementation step was installing Windows Server 2022 as a virtual machine in VMware Workstation Pro.

Microsoft provides an evaluation version through the [Windows Server 2022 Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) page.

The installation establishes the operating system that will later be configured with Active Directory Domain Services.

### Implementation sequence

1. Create the Windows Server 2022 virtual machine in VMware Workstation Pro.
2. Attach the Windows Server 2022 installation media.
3. Start the virtual machine and complete Windows Setup.
4. Configure the initial administrator account.
5. Log in to the Windows Server desktop.
6. Confirm that the server is operating normally before installing additional roles.

> **Screenshot placeholder:** Windows Server 2022 installation or first desktop after installation.

## 4.2 Initial Server Preparation

Before installing AD DS, the server should be prepared as the stable foundation for the domain environment.

The initial preparation includes:

- Confirming the server's network adapter is functioning.
- Assigning the planned static IP configuration.
- Confirming the server can communicate with the laboratory network.
- Giving the server a meaningful computer name if required by the laboratory naming convention.
- Confirming that Windows Server is ready for the AD DS role.

## 4.3 Configuring a Static IP Address

The Windows Server was configured with a static IP address. The exact value is intentionally documented as a placeholder:

```text
Server IP Address : [PLACEHOLDER]
Subnet Mask       : [PLACEHOLDER]
Default Gateway   : [PLACEHOLDER]
Preferred DNS     : [PLACEHOLDER]
```

A static IP is important because the server provides services that clients must consistently locate. Active Directory and DNS depend on stable server addressing, and changing the Domain Controller's address can introduce connectivity and name-resolution problems.

### Why the server should be static

- Clients need a predictable address for the Domain Controller and DNS service.
- DNS records should consistently point to the correct server.
- Troubleshooting is easier when the server address does not change.
- Other domain services can be configured around a known server address.

> **Screenshot placeholder:** Windows Server IPv4 properties showing the static IP configuration.

## 4.4 Verifying Server Connectivity

After configuring the static IP, basic network connectivity should be checked before continuing with the domain configuration.

A basic test from a client or another reachable system can use:

```cmd
ping [SERVER-IP]
```

A successful response indicates that basic IP communication is working. This test should not be confused with DNS or Active Directory verification; those require additional checks later in the implementation.

## 4.5 Preparing for Active Directory

Once the server network configuration was established, the server was ready to become the central identity and management server for the laboratory.

The next implementation stage is installing the **Active Directory Domain Services (AD DS)** role. Installing the role prepares Windows Server to provide Active Directory functionality, but the server must still be promoted to a Domain Controller before the domain is operational.

The overall transition is:

```text
Windows Server 2022
        │
        ▼
Static IP configured
        │
        ▼
Install AD DS role
        │
        ▼
Promote to Domain Controller
        │
        ▼
Create Yasha.local
```

> **Screenshot placeholder:** Server Manager before or during AD DS role installation.

## 4.6 Server Implementation Checklist

Before moving to the AD DS chapter, verify that:

- [ ] Windows Server 2022 is installed successfully.
- [ ] The server has the intended computer name.
- [ ] A static IP address has been configured.
- [ ] The server can communicate with the laboratory network.
- [ ] The server is ready for AD DS installation.

The next chapter documents the AD DS installation, Domain Controller promotion, domain creation, OU structure, and user-account configuration.
