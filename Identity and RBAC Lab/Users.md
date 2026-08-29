# Microsoft Entra ID Users

The first step of the lab was creating the test users that would be used to represent different responsibilities within the Azure environment.

Microsoft Entra ID was used to create and manage these identities. Each account was created separately so that I could later test how Azure RBAC permissions affected different users.

---

# Users Created

The following test accounts were created:

| User | Purpose |
|---|---|
| `cloudadmin` | Represents a cloud administration role |
| `networkadmin` | Represents a network administration role |
| `storageadmin` | Represents a storage administration role |
| `developer` | Represents a development role |
| `auditor` | Represents an auditing and read-only role |

An additional existing directory account was also present in the tenant, but it was not part of the RBAC lab users.

---

# Creating the Users

The users were created through the Microsoft Entra ID **Users** section in the Azure Portal.

The general process was:

```text
Azure Portal
    ↓
Microsoft Entra ID
    ↓
Users
    ↓
New User
    ↓
Create User
```

Each test account was given a username and configured as a member account within the directory.

The accounts were created separately because each account would later be associated with a specific security group and tested against different Azure permissions.

---

# User Structure

The users were designed around different responsibilities:

```text
cloudadmin
    ↓
Cloud Administration

networkadmin
    ↓
Network Administration

storageadmin
    ↓
Storage Administration

developer
    ↓
Development

auditor
    ↓
Auditing
```

These responsibilities were later used to determine which security groups and Azure RBAC permissions each user should receive.

---

# Verification

After creating the accounts, I verified that the users appeared in the Microsoft Entra ID **All users** section.

The Azure Portal displayed the five lab accounts:

```text
auditor
cloudadmin
developer
networkadmin
storageadmin
```

<img width="1640" height="508" alt="Entra ID Lab Users pic" src="https://github.com/user-attachments/assets/a5b0e28b-7b84-4ba6-93a6-2af0ef9f21bc" />

---

# Why Separate Test Accounts?

Creating separate accounts made it possible to test Azure RBAC from the perspective of different users.

Instead of testing everything from one administrator account, each account could be used to determine whether the permissions assigned to its corresponding role were working correctly.

This became especially important later in the lab when testing whether users could access or modify resources outside of their assigned responsibilities.

---

# Result

The required Microsoft Entra ID test users were successfully created and verified.

The next step was to organize these users into security groups based on their responsibilities.
