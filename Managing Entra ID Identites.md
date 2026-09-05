# AZ-104 Lab 01 – Manage Microsoft Entra ID Identities

Hands-on Microsoft Azure administration lab focused on managing **Microsoft Entra ID users, guest accounts, security groups, and group memberships**.

This lab demonstrates how identity objects can be created and organized to support access management within an Azure environment.

---

##  Objectives

- Create and configure Microsoft Entra ID user accounts
- Configure user properties such as job title and department
- Invite an external guest user
- Create a security group
- Assign users to a security group
- Review group membership and properties
- Understand the difference between internal and external users
- Understand assigned vs. dynamic group membership

---

##  Scenario

An organization is building a lab environment for pre-production testing of applications and services.

Several engineers need to manage the lab environment and authenticate using Microsoft Entra ID.

To support this environment, users and groups must be provisioned and organized appropriately.

The lab focuses on two primary administrative tasks:

1. Creating and configuring user accounts
2. Creating security groups and assigning members

---

##  Technologies Used

- Microsoft Azure
- Microsoft Entra ID
- Microsoft Entra Admin Center
- Security Groups
- Guest/B2B Users

---

# Task 1 – Create and Configure User Accounts

## 1. Create Internal User

Created an internal Microsoft Entra ID user:

| Setting | Configuration |
|---|---|
| User Principal Name | `az-104-user1` |
| Display Name | `az-104-user1` |
| Job Title | `IT Lab Administrator` |
| Department | `IT` |
| Account Status | Enabled |
| Password | Auto-generated |

### User Creation

<img width="958" height="842" alt="AZ-104 Entra Project User creation" src="https://github.com/user-attachments/assets/85f50335-180a-45b9-b15f-e9e61be08d8b" />

The user was created as an internal member account within the Microsoft Entra tenant.

---

## 2. Configure User Properties

Additional identity and organizational properties were reviewed and configured.

<img width="961" height="845" alt="az-104 user creation properties" src="https://github.com/user-attachments/assets/b8133657-338b-42c7-b72a-033b4748e07e" />

The **IT Lab Administrator** job title and **IT** department were configured to represent the user's role within the lab environment.

---

# Task 2 – Invite an External User

Microsoft Entra ID also supports external users through guest/B2B collaboration.

An external user was invited using their personal email address.

### Invitation Configuration

<img width="959" height="866" alt="az-104 user invitation" src="https://github.com/user-attachments/assets/5849b92b-2f20-4e7a-b673-f977962dd203" />

The invitation included a custom message:

> Welcome to Azure and our group project.

The invitation was then sent to the external account.

---

## 3. Verify Guest Invitation

The external user received an invitation email from the Microsoft Entra tenant.

<img width="655" height="718" alt="az-104 user invitation email" src="https://github.com/user-attachments/assets/0d0011a9-e8fe-4f8a-a50e-2878999616b8" />

This demonstrates the external user invitation workflow used by Microsoft Entra ID.

---

# Task 3 – Create Security Group

A security group named:

**IT Lab Administrators**

was created to organize users responsible for managing the lab environment.

### Group Configuration

| Setting | Configuration |
|---|---|
| Group Type | Security |
| Group Name | `IT Lab Administrators` |
| Membership Type | Assigned |
| Description | `Administrators that manage the IT lab` |

The group uses **Assigned** membership, meaning administrators manually manage which users belong to the group.

---

## 4. Add Group Members

The following accounts were added to the security group:

- `az-104-user1`
- External guest user

<img width="959" height="867" alt="az-104 IT Lab group members assigned" src="https://github.com/user-attachments/assets/8db419ef-51d0-458f-a66c-4d027099c606" />

The Members page was used to verify that both accounts were successfully added.

---

## 5. Review Group Properties

The completed security group was reviewed in Microsoft Entra ID.

<img width="961" height="842" alt="az-104 IT Lab administators creation" src="https://github.com/user-attachments/assets/9b9a2612-21ae-42dd-9ca7-0066500802fe" />

The group is configured as a **Security** group with **Assigned** membership.

---

#  Identity & Access Concepts

## Internal Users

Internal users represent accounts belonging to the organization's Microsoft Entra tenant.

They can be assigned permissions, added to groups, and used to authenticate against organizational resources.

## External / Guest Users

Guest users allow external individuals to collaborate with an organization while maintaining a separate external identity.

This is useful for scenarios involving:

- Contractors
- Partners
- Consultants
- External collaborators

## Security Groups

Security groups allow administrators to organize users and assign access based on group membership rather than configuring every user individually.

For example:

```text
IT Lab Administrators
│
├── az-104-user1
└── Guest User
