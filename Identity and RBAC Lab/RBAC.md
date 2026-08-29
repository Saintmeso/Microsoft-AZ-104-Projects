# Azure Role-Based Access Control (RBAC)

After creating the Microsoft Entra ID users, security groups, and Azure resource groups, the next step was configuring Azure Role-Based Access Control (RBAC).

RBAC was used to control what each group could do within the Azure environment.

The main concept I focused on was:

```text
WHO → WHAT → WHERE
```

- **WHO** → The user or group receiving access
- **WHAT** → The Azure RBAC role being assigned
- **WHERE** → The scope where the role applies

---

# Configuring Role Assignments

RBAC role assignments were configured through the **Access Control (IAM)** section of the Azure resource groups.

The general process was:

```text
Resource Group
      ↓
Access Control (IAM)
      ↓
Add Role Assignment
      ↓
Select Role
      ↓
Select Members
      ↓
Review + Assign
```

Rather than assigning roles directly to individual users, I assigned roles to the security groups created earlier.

This allowed the users to inherit permissions through their group membership.

---

# Compute Resource Group

The `rg-compute` resource group was used to configure and document the individual role assignments in detail.

The following groups were assigned roles within the compute resource group:

```text
Cloud-Admins
Developers
Network-Admins
Storage-Admins
Auditors
```

Each assignment was selected based on the responsibilities of the group and the level of access required.

---

## Cloud-Admins

The Cloud-Admins group was given permissions related to managing compute resources within `rg-compute`.

<img width="1919" height="802" alt="RG Cloud Assignment" src="https://github.com/user-attachments/assets/ecf0213e-08cf-41bb-924f-77c49f21b37c" />

---

## Developers

The Developers group was given permissions related to their development responsibilities within `rg-compute`.

<img width="1919" height="801" alt="RG Developer Assignment" src="https://github.com/user-attachments/assets/862f06a6-8ae4-4da3-aa4e-877a116734b8" />

---

## Network-Admins

The Network-Admins group was assigned permissions within the compute resource group that were relevant to their responsibilities.

<img width="1901" height="778" alt="RG Network Assignment" src="https://github.com/user-attachments/assets/f9d9a5f5-c041-44bf-9a2f-66769deaa185" />

---

## Storage-Admins

The Storage-Admins group was assigned permissions within the compute resource group that were relevant to their responsibilities.

<img width="1918" height="788" alt="RG Storage Assignment" src="https://github.com/user-attachments/assets/da6d81a2-4317-419f-9ff2-9539012795ee" />

---

## Auditors

The Auditors group was given read-only access where required.

The Reader role was appropriate for auditing because auditors primarily need to view resources, configurations, and available information rather than make administrative changes.

<img width="1917" height="801" alt="RG Auditor Assignment" src="https://github.com/user-attachments/assets/d542623b-b4e7-48b8-aca4-d632ab1454ef" />

---

# Network Resource Group

The `rg-network` resource group was configured separately from the compute environment.

The final role assignments were reviewed through the resource group's **Access Control (IAM)** section.

<img width="1331" height="812" alt="rgnetwork Role Assignments" src="https://github.com/user-attachments/assets/ec72785c-7bb4-4ac1-8d04-a6ae8c5cf73f" />

The purpose of separating the network resource group was to allow networking permissions to be managed independently from compute and storage permissions.

This allowed the `Network-Admins` group to receive network-specific access without automatically providing the same level of access to unrelated resources.

---

# Storage Resource Group

The `rg-storage` resource group was also configured separately.

The final role assignments were reviewed through the resource group's **Access Control (IAM)** section.

<img width="1907" height="822" alt="rgstorage role assignments" src="https://github.com/user-attachments/assets/0a3e24a5-a4d6-4c8d-be1b-7db9ef8ec19f" />

The storage permissions were separated from networking permissions so that Storage-Admins could manage the resources they were responsible for without automatically receiving network-management privileges.

---

# Role Assignment Structure

The overall RBAC structure can be represented as:

```text
Microsoft Entra ID
       │
       ▼
Security Groups
       │
       ▼
Azure RBAC Role
       │
       ▼
Resource Group Scope
```

For example:

```text
Network-Admins
      ↓
Network Contributor
      ↓
rg-network
```

Another example:

```text
Storage-Admins
      ↓
Storage Role
      ↓
rg-storage
```

---

# Least-Privilege Approach

A major goal of the RBAC configuration was to avoid giving every group unrestricted access to the entire Azure subscription.

Instead, permissions were based on the responsibilities of each group.

For example:

```text
Network-Admins → Network resources
Storage-Admins → Storage resources
Auditors → Read-only access
Developers → Development resources
Cloud-Admins → Compute administration
```

This approach follows the principle of least privilege.

The goal is to give users enough access to perform their responsibilities without unnecessarily granting permissions to unrelated resources.

---

# Why Assign Roles to Groups?

Roles were assigned to groups rather than individual users.

For example:

```text
storageadmin
      ↓
Storage-Admins
      ↓
Storage RBAC Role
      ↓
rg-storage
```

This is easier to manage than assigning permissions individually.

If another user needs the same access, they can be added to the appropriate security group instead of creating another individual role assignment.

---

# RBAC Scope

Scope determines where an RBAC role assignment applies.

The Azure hierarchy can be represented as:

```text
Management Group
       ↓
Subscription
       ↓
Resource Group
       ↓
Resource
```

For this lab, resource-group-level scope was primarily used.

For example:

```text
Network-Admins
      ↓
Network Contributor
      ↓
rg-network
```

The role assignment is therefore associated with the `rg-network` scope.

---

# Role Selection Challenge

One of the most difficult parts of configuring RBAC was selecting the appropriate role.

Azure provides a large number of built-in roles, and many contain similar names.

For example, searching for:

```text
Contributor
```

returned many different specialized roles.

This made it important to look beyond the role name and examine the actual permissions provided by the role.

I learned that a role should be selected based on:

- What resources the group needs to manage
- What actions the group needs to perform
- What actions the group should not be able to perform
- Where the permissions need to apply
- Whether the role provides more access than necessary

---

# Result

Azure RBAC was successfully configured across the compute, network, and storage environments.

The final configuration provided different levels of access based on each group's responsibilities.

The next step was testing these permissions using the individual test accounts to verify that the RBAC configuration actually enforced the intended access boundaries.
