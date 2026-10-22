# AZ-104 - Implementing Intersite Connectivity

This Azure lab is focused on connecting seperate VNets using VNet Peering and then testing connectivity with Azure Network Watcher and Powershell, as well as implementing custom routes using UDRs

## Scenario

An organization separates core IT services from its manufacturing network to improve network segmentation and manage access between different business environments.

However, certain systems in the core services network must communicate with resources in the manufacturing network.

To support this requirement, Azure Virtual Network Peering is configured to establish private connectivity between the two networks. Network Watcher and PowerShell are then used to validate connectivity, and a custom route is created to demonstrate traffic forwarding through a future Network Virtual Appliance (NVA).

The lab focuses on six primary tasks:

1. Creating a core services virtual machine and virtual network.
2. Creating a manufacturing virtual machine and a separate virtual network.
3. Testing connectivity using Azure Network Watcher.
4. Configuring Virtual Network Peering.
5. Validating connectivity using PowerShell.
6. Creating a custom route and associating it with a subnet.

## Architecture Overview

| Component | Core Services | Manufacturing |
|---|---|---|
| Virtual Network | `CoreServicesVnet` | `ManufacturingVnet` |
| Address Space | `10.0.0.0/16` | `172.16.0.0/16` |
| Subnet | `10.0.0.0/24` | `172.16.0.0/24` |
| Virtual Machine | `CoreServicesVM` | `ManufacturingVM` |
| Private IP Address | `10.0.0.4` | `172.16.0.4` |
| Region | East US | East US |

A separate perimeter subnet was created within `CoreServicesVnet` for the custom routing configuration.

| Component | Configuration |
|---|---|
| Perimeter Subnet | `perimeter` |
| Subnet Address Range | `10.0.1.0/24` |
| Route Table | `rt-CoreServices` |
| Custom Route | `PerimetertoCore` |
| Destination Network | `10.0.0.0/16` |
| Next Hop Type | Virtual appliance |
| Next Hop IP | `10.0.1.7` |

### Why Separate Core Services?

Separating core IT services from other business networks allows administrators to organize resources into distinct network boundaries and apply routing and access controls between environments.

### Core Services VM Configuration

<img width="896" height="1051" alt="az-104 connectivity vm creation" src="https://github.com/user-attachments/assets/40386ea4-520f-4ce0-8cfa-1884e6a1684f" />

## 2. Create CoreServicesVnet

Configured a virtual network named `CoreServicesVnet` with the address space `10.0.0.0/16`.

A subnet using the `10.0.0.0/24` address range was created for the core services virtual machine.

| Setting | Configuration |
|---|---|
| Virtual Network | `CoreServicesVnet` |
| Address Space | `10.0.0.0/16` |
| Subnet Address Range | `10.0.0.0/24` |
| Region | East US |

### Why Use a Dedicated Address Space?

A dedicated address space helps isolate the core services environment from other networks and makes routing between network segments easier to manage.

The address space must not overlap with the address space of a network being peered.

### Core Services Virtual Network Configuration

<img width="1277" height="666" alt="az-104 vnet creation 1" src="https://github.com/user-attachments/assets/2b86dca4-4fe7-471e-a069-c47b6ad7ffac" />

<img width="1126" height="1183" alt="az-104 connectivity subnet pic" src="https://github.com/user-attachments/assets/d9963d4b-31ca-4dfa-b422-ac300d4c9187" />

---

# Task 2 - Create the Manufacturing Virtual Network

## 1. Configure ManufacturingVM

Created a second virtual machine named `ManufacturingVM` in the East US region.

The virtual machine uses Windows Server 2025 Datacenter, x64 Gen2, with a Standard D2s v3 instance size.

| Setting | Configuration |
|---|---|
| Resource Group | `az104-rg5` |
| Virtual Machine | `ManufacturingVM` |
| Region | East US |
| Operating System | Windows Server 2025 Datacenter |
| VM Size | Standard D2s v3 |
| Administrator Username | `localadmin` |
| Public Inbound Ports | None |

The manufacturing virtual machine was placed in a separate virtual network to demonstrate communication between isolated Azure network environments.

### Manufacturing VM Configuration

<img width="890" height="967" alt="az-104 manufacturingVM creation" src="https://github.com/user-attachments/assets/2367f234-829e-459d-8730-487cfe17d3bb" />
## 2. Create ManufacturingVnet

Configured a second virtual network named `ManufacturingVnet`.

| Setting | Configuration |
|---|---|
| Virtual Network | `ManufacturingVnet` |
| Address Space | `172.16.0.0/16` |
| Subnet | `Manufacturing` |
| Subnet Address Range | `172.16.0.0/24` |
| Region | East US |

The manufacturing network uses a different private IPv4 address range from the core services network. This avoids overlapping address spaces and allows the networks to be connected through peering.

### Manufacturing Virtual Network Configuration

<img width="1274" height="649" alt="az-104 manufacturingVNet and Subnet" src="https://github.com/user-attachments/assets/5ddddbd6-93ca-48c9-b601-f916ff3b6fda" />

---

# Task 3 - Test Connectivity Using Azure Network Watcher

## 1. Configure Connection Troubleshoot

Azure Network Watcher was used to test connectivity between the two virtual machines before configuring Virtual Network Peering.

| Setting | Configuration |
|---|---|
| Source VM | `CoreServicesVM` |
| Destination VM | `ManufacturingVM` |
| Protocol | TCP |
| Destination Port | `3389` |
| Preferred IP Version | Both |
| Diagnostic Tests | Connectivity, NSG diagnostic, Next hop, Port scanner |

TCP port `3389` is commonly used for Remote Desktop Protocol (RDP).

### Connection Troubleshoot Configuration

<img width="1276" height="957" alt="az-104 networking setup test" src="https://github.com/user-attachments/assets/7cbaa2f0-6a3c-4b50-85b9-9e48b248ada3" />

## 2. Review the Initial Connectivity Results

The initial diagnostic test reported that connectivity was unreachable.

The results identified denied traffic in the network security group diagnostics for both virtual machines.

| Diagnostic | Result |
|---|---|
| Connectivity Test | Unreachable |
| Outbound NSG Diagnostic | Deny |
| Inbound NSG Diagnostic | Deny |
| Next Hop | Success |
| Destination Port Diagnostic | Reachable |

### Why Did the Initial Test Fail?

The virtual machines were deployed in separate virtual networks without configured peering. Additionally, the diagnostic results identified network security group restrictions.

Virtual Network Peering provides a private network path between virtual networks, but it does not automatically override network security group rules or Windows Firewall settings.

This initial test demonstrated the importance of checking routing, network security groups, and host-based firewall rules when troubleshooting connectivity.

### Initial Network Watcher Results

<img width="919" height="452" alt="az-104 networking results pic" src="https://github.com/user-attachments/assets/f843e7bb-65aa-4c96-b6cb-6e52eed45be1" />

---

# Task 4 - Configure Virtual Network Peering

## 1. Configure Bidirectional Peering

Created Virtual Network Peering between `CoreServicesVnet` and `ManufacturingVnet`.

| Setting | Configuration |
|---|---|
| Local Virtual Network | `CoreServicesVnet` |
| Remote Virtual Network | `ManufacturingVnet` |
| Core-to-Manufacturing Peering | `CoreServicesVnet-to-ManufacturingVnet` |
| Manufacturing-to-Core Peering | `ManufacturingVnet-to-CoreServicesVnet` |
| Allow Virtual Network Access | Enabled |
| Allow Forwarded Traffic | Enabled |

Peering was configured in both directions so that each virtual network has a corresponding peering relationship with the other.

### Why Virtual Network Peering?

Virtual Network Peering connects Azure virtual networks using Microsoft's backbone network. Resources can communicate using their private IP addresses, subject to applicable routing and security rules.

Peering is useful when an organization needs to connect separate environments, such as:

- Core IT services and manufacturing systems.
- Production and development environments.
- Shared services and application networks.
- Separate departmental or business-unit networks.

### Virtual Network Peering Configuration

<img width="1266" height="1149" alt="az-104 Vnet peering pic" src="https://github.com/user-attachments/assets/d1d926c8-59c0-44f4-8138-6296ac9a8500" />

## 2. Verify Peering Status

After creating the peering configuration, the peering relationship was checked in the Azure portal.

The ManufacturingVnet peering screenshot shows a connected peering relationship and a fully synchronized status.

### Peering Verification

<img width="1274" height="374" alt="az-104 ensuring ManufactuingVnet is peered" src="https://github.com/user-attachments/assets/996dc424-86d2-454a-9ad1-e45e3f9bc6f5" />

---

# Task 5 - Validate Connectivity Using PowerShell

## 1. Configure the Core Services Windows Firewall

The Remote Desktop firewall rule was enabled on `CoreServicesVM` using Azure Run Command.

```powershell
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

This command enables the Windows Firewall rules associated with Remote Desktop, allowing RDP traffic when network connectivity and other security rules permit it.

### Enable Remote Desktop Firewall Rule

![Enable Remote Desktop firewall rule](images/12-enable-rdp-firewall.png)

## 3. Test the TCP Connection

After configuring the firewall rule, I used PowerShell on `ManufacturingVM` to test connectivity to `CoreServicesVM` over TCP port `3389`.

```powershell
Test-NetConnection 10.0.0.4 -Port 3389
```

The command checks whether `ManufacturingVM` can establish a TCP connection to the private IP address of `CoreServicesVM`.

| Setting | Value |
|---|---|
| Source VM | `ManufacturingVM` |
| Source Private IP | `172.16.0.4` |
| Destination VM | `CoreServicesVM` |
| Destination Private IP | `10.0.0.4` |
| Protocol | TCP |
| Destination Port | `3389` |

### PowerShell Connectivity Test

<img width="1277" height="608" alt="az-104 running command on Coreservicesvm" src="https://github.com/user-attachments/assets/ddcbdc3b-a530-48b7-a959-39db42797f3f" />

### Connectivity Test Results

The test returned the following results:

```text
SourceAddress    : 172.16.0.4
TcpTestSucceeded : True
```

!<img width="2022" height="778" alt="PowerShell Test-NetConnection Results" src="https://github.com/user-attachments/assets/933e77db-d4da-4247-b523-9d06064b9dbb" />

### Result

The successful TCP test confirmed that `ManufacturingVM` could reach `CoreServicesVM` on TCP port `3389` using its private IP address. This verified connectivity between the two peered virtual networks.

---

# Task 6 - Create a Custom Route Using a User-Defined Route (UDR)

## 1. Create a Perimeter Subnet

A separate subnet named `perimeter` was created within `CoreServicesVnet` for the custom routing configuration.

| Setting | Configuration |
|---|---|
| Virtual Network | `CoreServicesVnet` |
| Subnet Name | `perimeter` |
| Address Range | `10.0.1.0/24` |

### Perimeter Subnet Configuration

<img width="1277" height="911" alt="az-104 perimeter subnet creation" src="https://github.com/user-attachments/assets/da8b1683-588f-4278-a4d0-596270c49f0c" />

### Why Create a Perimeter Subnet?

A perimeter subnet can be used to place network security appliances between network segments. In a production environment, this can support centralized traffic inspection and controlled traffic forwarding.

## 2. Create the Route Table

Created an Azure route table named `rt-CoreServices` in the East US region.

| Setting | Configuration |
|---|---|
| Route Table Name | `rt-CoreServices` |
| Resource Group | `az104-rg5` |
| Region | East US |
| Propagate Gateway Routes | Yes |

### Route Table Creation

<img width="777" height="524" alt="az-104 route tables creation" src="https://github.com/user-attachments/assets/ed52b98b-6cab-4756-a09e-037d67bbb4d8" />

## 3. Configure the Custom Route

Added a custom route named `PerimetertoCore` to direct traffic destined for the core services address space to a virtual appliance next hop.

| Setting | Configuration |
|---|---|
| Route Name | `PerimetertoCore` |
| Destination Address Prefix | `10.0.0.0/16` |
| Next Hop Type | Virtual appliance |
| Next Hop IP Address | `10.0.1.7` |

### Custom Route Configuration

<img width="854" height="529" alt="az-104 adding route" src="https://github.com/user-attachments/assets/db1f428c-1b08-43b1-9f3b-2d51c54e5fef" />

### Why Use a User-Defined Route?

User-defined routes (UDRs) allow administrators to customize how traffic is routed within Azure virtual networks. Instead of relying exclusively on Azure's default system routes, administrators can direct traffic through a specified next hop.

Virtual appliance next hops can support scenarios involving:

- Network traffic inspection.
- Centralized firewall enforcement.
- Traffic filtering between network segments.
- Routing traffic through a network virtual appliance (NVA).

In this lab, `10.0.1.7` was configured as the virtual appliance next hop to demonstrate custom routing.

**Note:** The route configuration alone does not confirm that an NVA is deployed at `10.0.1.7` or that traffic is successfully forwarded through it. The appliance would need to be deployed and tested separately.

## 4. Associate the Route Table with the Subnet

The `rt-CoreServices` route table was associated with the `perimeter` subnet.

### Route Table Association

<img width="585" height="287" alt="az-104 associate subnet" src="https://github.com/user-attachments/assets/b2d2f135-1a27-45f6-bfc4-a5b8065f30c8" />

## 5. Validate the Routing Configuration

After creating the route table and custom route, I verified the configuration in the Azure portal.

The route table contained the `PerimetertoCore` route with the destination `10.0.0.0/16` and the virtual appliance next hop `10.0.1.7`.

### Result

The custom route was configured and associated with the perimeter subnet. This demonstrated how Azure administrators can use UDRs to control traffic paths within a virtual network.

---

# Key Takeaways

- **Virtual Network Peering:** Connected two Azure virtual networks with non-overlapping address spaces.
- **Private Connectivity:** Tested communication between virtual machines using private IP addresses.
- **Network Troubleshooting:** Used Azure Network Watcher to investigate connectivity and network security group behavior.
- **PowerShell Testing:** Used `Test-NetConnection` to validate TCP connectivity on port `3389`.
- **Windows Firewall:** Enabled the Remote Desktop firewall rules on `CoreServicesVM`.
- **User-Defined Routes:** Created a route table and configured a virtual appliance next hop.
- **Subnet Association:** Associated the route table with the perimeter subnet.

# Conclusion

This lab provided hands-on experience implementing intersite connectivity in Microsoft Azure. I created separate virtual networks for core services and manufacturing, configured bidirectional VNet peering, and validated private connectivity using PowerShell.

I also used Azure Network Watcher to investigate connectivity issues and configured a user-defined route to demonstrate how traffic can be directed through a virtual appliance.

These skills are relevant to Azure network administration, cloud infrastructure management, and troubleshooting connectivity and routing in segmented network environments.

# Technologies Used

- Microsoft Azure
- Azure Virtual Machines
- Azure Virtual Networks
- Virtual Network Peering
- Azure Network Watcher
- Network Security Groups (NSGs)
- Azure Route Tables
- User-Defined Routes (UDRs)
- Windows Server
- PowerShell
