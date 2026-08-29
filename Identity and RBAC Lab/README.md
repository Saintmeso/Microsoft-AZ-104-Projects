# Enterprise Azure Identity & RBAC Lab

Hands-on Microsoft Azure lab focused on **Microsoft Entra ID, Azure Role-Based Access Control (RBAC), security groups, resource groups, and least-privilege access**.

The purpose of this lab was to gain practical experience managing identities and permissions in Azure by creating users and groups, organizing Azure resources, assigning RBAC roles, and testing whether users could access resources based on their assigned permissions.

---

# Lab Objectives

The main objectives of this lab were to:

- Create Microsoft Entra ID users
- Create security groups
- Assign users to appropriate groups
- Create and organize Azure resource groups
- Configure Azure RBAC role assignments
- Understand RBAC roles and permissions
- Understand RBAC scope
- Apply the principle of least privilege
- Assign permissions to groups instead of individual users
- Test permissions using separate user accounts
- Troubleshoot authorization and access issues

---

# Architecture

The lab was designed around separating users, groups, Azure resources, and permissions.

```text
Microsoft Entra ID
│
├── Users
│   ├── cloudadmin
│   ├── networkadmin
│   ├── storageadmin
│   ├── developer
│   └── auditor
│
└── Security Groups
    ├── Cloud-Admins
    ├── Network-Admins
    ├── Storage-Admins
    ├── Developers
    └── Auditors
             │
             ▼
        Azure RBAC
             │
             ▼
       Role Assignments
             │
       ┌─────┼─────┐
       ▼     ▼     ▼
    Compute Network Storage
       │     │     │
       ▼     ▼     ▼
  rg-compute rg-network rg-storage
```

The RBAC model used throughout the lab can be summarized as:

```text
WHO → WHAT → WHERE
```

- **WHO** → User or security group receiving access
- **WHAT** → RBAC role being assigned
- **WHERE** → Scope where the permissions apply

---

# Lab Environment

### Microsoft Entra ID

Five test users were created:

```text
cloudadmin
networkadmin
storageadmin
developer
auditor
```

These users were organized into five security groups:

```text
Cloud-Admins
Network-Admins
Storage-Admins
Developers
Auditors
```

### Azure Resource Groups

Three resource groups were created:

```text
rg-compute
rg-network
rg-storage
```

These resource groups were used to separate compute, networking, and storage-related resources and permissions.

---

# RBAC Design

The lab used group-based Azure RBAC assignments rather than assigning permissions individually to every user.

The general structure was:

```text
User
  ↓
Security Group
  ↓
RBAC Role
  ↓
Resource Group
```

This allowed permissions to be managed according to the responsibilities of each group.

Examples included:

```text
Auditors
    ↓
Reader
    ↓
Resource Group
```

```text
Network-Admins
    ↓
Network Contributor
    ↓
rg-network
```

```text
Storage-Admins
    ↓
Storage Account Contributor
    ↓
rg-storage
```

The goal was to provide each group with the permissions necessary for its responsibilities without unnecessarily granting access to unrelated resources.

---

# Permission Testing

After configuring the RBAC assignments, separate test accounts were used to verify the permissions.

The tests included:

- Testing whether the Storage Administrator could create a Virtual Network
- Verifying Network Administrator permissions
- Verifying Storage Administrator permissions
- Verifying Auditor read-only access

Testing was important because the goal was not simply to configure RBAC, but to verify that the permissions actually behaved as expected.

One important result occurred when the Storage Administrator attempted to create a Virtual Network. The configuration passed validation, but the actual deployment failed because the account did not have the required network permissions.

This demonstrated the importance of testing actual operations when validating access controls.

---

# Documentation

The individual parts of the lab are documented separately:

### [Users](./Users.md)

Documents the creation of the Microsoft Entra ID test users.

### [Groups](./Groups.md)

Documents the creation of security groups and assigning users to those groups.

### [Resource Groups](./Resource-Groups.md)

Documents the creation of the Azure resource groups used in the lab.

### [RBAC](./RBAC.md)

Documents the Azure RBAC configuration, role assignments, permissions, and scope.

### [Testing](./Testing.md)

Documents the permission tests performed using the different test accounts.

### [Challenges](./Challenges.md)

Documents the problems encountered during the lab, including selecting appropriate RBAC roles, understanding scope, and troubleshooting authorization failures.

---

# Technologies

| Category | Technology |
|---|---|
| Cloud Platform | Microsoft Azure |
| Identity | Microsoft Entra ID |
| Access Control | Azure RBAC |
| Resource Management | Azure Resource Groups |
| Administration | Azure Portal |
| Testing | Azure Test Accounts |

---

# Skills Demonstrated

- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Identity & Access Management
- Role-Based Access Control
- Least Privilege
- Security Groups
- Resource Group Management
- Authorization
- Permission Testing
- Cloud Administration
- Troubleshooting

---

# Project Status

**Completed**

The Azure identity and RBAC environment was configured and tested using dedicated accounts to verify that users received the appropriate permissions based on their assigned groups and roles.
