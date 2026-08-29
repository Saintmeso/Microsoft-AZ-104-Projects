# Microsoft Entra ID Security Groups

After creating the users, the next step was to organize them into security groups based on their responsibilities.

Using groups makes it easier to manage permissions because Azure RBAC roles can be assigned to a group instead of having to assign the same permissions to every user individually.

---

# Groups Created

The following security groups were created:

```text
Cloud-Admins
Network-Admins
Storage-Admins
Developers
Auditors
```

Each group represents a different responsibility within the Azure environment.

| Security Group | Responsibility |
|---|---|
| `Cloud-Admins` | Cloud and compute administration |
| `Network-Admins` | Network resource administration |
| `Storage-Admins` | Storage resource administration |
| `Developers` | Development and compute-related activities |
| `Auditors` | Read-only access for auditing |

---

# Creating the Groups

The groups were created through the Microsoft Entra ID **Groups** section in the Azure Portal.

The general process was:

```text
Azure Portal
    ↓
Microsoft Entra ID
    ↓
Groups
    ↓
New Group
    ↓
Create
```

Each group was created as a security group.

---

# Assigning Users to Groups

After creating the groups, the test users were added to their corresponding groups.

The basic structure was:

```text
cloudadmin
    ↓
Cloud-Admins

networkadmin
    ↓
Network-Admins

storageadmin
    ↓
Storage-Admins

developer
    ↓
Developers

auditor
    ↓
Auditors
```

This created the identity structure that would later be used when assigning Azure RBAC roles.

---

# Why Use Groups?

Using groups instead of assigning permissions directly to individual users makes access management easier to maintain.

For example, instead of assigning a storage role separately to every storage administrator:

```text
User → Storage Role
User → Storage Role
User → Storage Role
```

the role can be assigned to:

```text
Storage-Admins
       ↓
Storage Role
```

Any user who is a member of the group can then receive the permissions associated with that role.

This also makes it easier to manage access when users change responsibilities or new users are added.

---

# Verification

After creating the groups and adding the users, I verified that the groups appeared in Microsoft Entra ID and that the users were organized into their appropriate groups.

<img width="1629" height="627" alt="Entra ID Lab Groups Pic" src="https://github.com/user-attachments/assets/70d0ebc2-8711-4907-98d2-f0a229c0258e" />

The completed group structure provided the foundation for the next stage of the lab: configuring Azure resource groups and RBAC permissions.

---

# Result

The Microsoft Entra ID security groups were successfully created and organized around the different responsibilities in the lab.

The next step was creating the Azure resource groups that would later be used as RBAC scopes.
