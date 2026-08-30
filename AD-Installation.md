# Building an Active Directory Domain Controller in a Windows Server 2022 Virtual Machine
## Introduction

As part of my cybersecurity lab environment, I set up a Windows Server 2022 virtual machine and configured it to become an Active Directory Domain Controller. The goal of this project is to build a small, isolated enterprise-style environment where I can practice identity and access management (IAM), Windows administration, Active Directory, user provisioning, group management, and domain-based authentication.
For this lab, I used **Oracle VirtualBox** to host the virtual machine and **Windows Server 2022 Standard (Server Core)** as the operating system.
The final environment will use a domain called:

`corp.lab`

and the Domain Controller will be named:

`DC01`
---
## 1. Creating the Windows Server Virtual Machine

I started by creating a virtual machine in Oracle VirtualBox and installing Windows Server 2022 Standard.
Unlike the desktop version of Windows Server, I installed the **Server Core** version. Server Core does not include the traditional graphical Windows desktop. Instead, system administration is primarily performed through PowerShell and Microsoft's Server Configuration tool (`SConfig`).
After logging into the server, I was presented with the SConfig interface.

<img width="1022" height="851" alt="image" src="https://github.com/user-attachments/assets/494f540c-bc36-48ce-96b2-61e729d2f24b" />

**[SCREENSHOT — Windows Server 2022 SConfig screen]**

*Figure 1: Initial Windows Server 2022 Server Core configuration screen.*

The SConfig menu provides several basic server management options, including computer naming, networking, Windows updates, remote management, and restarting or shutting down the server.

---

## 2. Accessing PowerShell
Because Server Core does not provide the normal Windows desktop interface, I used PowerShell to perform the majority of the configuration.
From the SConfig menu, I selected option **15**, which exits SConfig and opens a PowerShell command prompt.
This gave me access to PowerShell commands that could be used to configure the server's network settings and eventually install Active Directory Domain Services.

**[SCREENSHOT — PowerShell prompt after exiting SConfig]**

*Figure 2: PowerShell environment used to configure the Windows Server.*

---

## 3. Identifying the Network Adapter
Before installing Active Directory, I needed to configure the server's network settings.
Active Directory relies heavily on networking and DNS, so having a predictable IP address is important for a Domain Controller.
I first checked the available network adapters using:

```powershell
Get-NetAdapter
```

The server had one active Ethernet adapter:

* **Adapter:** Ethernet
* **Interface Index:** 2
* **Status:** Up
* **Link Speed:** 1 Gbps

**[SCREENSHOT — Get-NetAdapter output]**

*Figure 3: Identifying the active Ethernet network adapter.*

I then used the following command to view the current IP configuration:

```powershell
Get-NetIPConfiguration
```

Initially, the server was receiving its IPv4 address through DHCP. The server had an address of `10.0.2.15`, which is consistent with the default NAT networking configuration used by VirtualBox.

**[SCREENSHOT — Initial Get-NetIPConfiguration output]**

*Figure 4: Initial network configuration assigned to the Windows Server VM.*

---

## 4. Configuring a Static IP Address

A Domain Controller should use a static IP address because other systems in the environment need to reliably locate services such as Active Directory and DNS.
I disabled DHCP on the Ethernet interface using:
```powershell
Set-NetIPInterface -InterfaceIndex 2 -Dhcp Disabled
```

I then removed the dynamically assigned IPv4 address:

```powershell
Remove-NetIPAddress -InterfaceIndex 2 -AddressFamily IPv4 -Confirm:$false
```

Next, I assigned the server a static IP address:
```powershell
New-NetIPAddress -InterfaceIndex 2 -IPAddress 10.0.2.10 -PrefixLength 24 -DefaultGateway 10.0.2.2
```
For this lab, the network configuration became:

| Setting         | Value           |
| --------------- | --------------- |
| IPv4 Address    | `10.0.2.10`     |
| Subnet Prefix   | `/24`           |
| Subnet Mask     | `255.255.255.0` |
| Default Gateway | `10.0.2.2`      |

I also configured the DNS server:
```powershell
Set-DnsClientServerAddress -InterfaceIndex 2 -ServerAddresses 10.0.2.2
```

At this stage, the server had a predictable IP address that could be used by the future Active Directory environment.

**[SCREENSHOT — Final Get-NetIPConfiguration output]**

*Figure 5: Static IP configuration after configuring the Domain Controller's network interface.*

---
## 5. Why the Static IP Is Important
The static IP configuration is particularly important for Active Directory because Domain Controllers provide critical network services to domain clients.
For example, computers joining the domain need to be able to consistently communicate with the Domain Controller and locate Active Directory services.
In addition, Active Directory depends heavily on DNS. The Domain Controller will eventually host DNS for the `corp.lab` domain.
Using a static IP prevents the Domain Controller's address from changing unexpectedly.
---

## 6. Installing Active Directory Domain Services
Once the basic network configuration was complete, I installed the **Active Directory Domain Services (AD DS)** role.
I used PowerShell rather than the graphical Server Manager because this installation was performed on Server Core.
The installation command was:

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```
This installs the components necessary to configure the Windows Server as an Active Directory Domain Controller.

**[SCREENSHOT — Install-WindowsFeature command and successful installation]**

*Figure 6: Installing the Active Directory Domain Services role.*

The successful installation of AD DS means the server now has the necessary Active Directory components, but it is **not yet a Domain Controller**.
The next step is to promote the server and create the Active Directory forest.
---

## 7. Promoting the Server to a Domain Controller
After installing AD DS, the next stage is to create the Active Directory forest.
For this lab, I chose the internal domain:
`corp.lab`

The server will use the name:

`DC01`

The promotion process can be started using:

```powershell
Install-ADDSForest -DomainName "corp.lab"
```

This command creates a new Active Directory forest and promotes the Windows Server to the first Domain Controller in that forest.
During the process, Windows Server will also configure the required Active Directory and DNS services.

**[SCREENSHOT — Install-ADDSForest command]**

*Figure 7: Beginning the Domain Controller promotion process.*

The promotion process requires a **Directory Services Restore Mode (DSRM)** password. This password is used for specialized Active Directory recovery and maintenance operations.

**[SCREENSHOT — DSRM password prompt]**

*Figure 8: Configuring the Directory Services Restore Mode password.*
The server may display warnings related to DNS delegation and will request confirmation before continuing. These warnings are expected when creating a new Active Directory forest in a laboratory environment.
After confirming the installation, Windows Server configures the Active Directory forest and restarts the server.
---

## 8. Resulting Active Directory Environment
After the promotion process is complete, the server becomes the first Domain Controller for the new domain.
The basic lab environment will look like this:
```text
                VirtualBox
                    |
                    |
                  DC01
          Windows Server 2022
              10.0.2.10
                    |
              Active Directory
                    |
                 corp.lab
```
The Domain Controller will provide services including:
* Active Directory Domain Services
* DNS
* Domain authentication
* User and group management
* Computer account management
* Group Policy
* Kerberos authentication
* LDAP directory services
---

## 9. Future Lab Expansion
This Domain Controller will serve as the foundation for a larger cybersecurity and IAM laboratory.
The next stage will be to create a Windows client virtual machine and join it to the `corp.lab` domain.
From there, I can create organizational units, users, security groups, and service accounts.
The planned environment will eventually resemble:

```text
corp.lab
│
├── Users
│   ├── Alice
│   ├── Bob
│   └── Administrator
│
├── Groups
│   ├── IT
│   ├── HR
│   └── Security
│
├── Computers
│   ├── DC01
│   └── CLIENT01
│
└── Service Accounts
    ├── svc_backup
    └── svc_application
```

This will allow me to practice real-world IAM concepts such as **Joiner-Mover-Leaver (JML) processes, provisioning, deprovisioning, role-based access control, group-based permissions, privileged accounts, service accounts, Group Policy, and access auditing**.

---

## Conclusion

This project demonstrates the initial setup of an Active Directory environment using Windows Server 2022 running inside Oracle VirtualBox.

I began with a fresh Windows Server 2022 Server Core installation, accessed PowerShell through SConfig, identified the network interface, configured a static IP address, and installed the Active Directory Domain Services role.

The next phase is promoting the server to a Domain Controller and creating the `corp.lab` Active Directory forest. Once that is complete, the environment can be expanded with Windows client machines, users, groups, organizational units, and IAM-focused workflows.

This lab provides a controlled environment for developing practical Windows administration, Active Directory, networking, and cybersecurity skills without requiring a production environment.
