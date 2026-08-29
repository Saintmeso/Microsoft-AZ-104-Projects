# Azure Resource Groups

After creating the Microsoft Entra ID users and security groups, the next step was to create the Azure resource groups that would be used to organize the lab environment and later serve as RBAC scopes.

---

# Resource Groups Created

Three resource groups were created:

```text
rg-compute
rg-network
rg-storage
```

Each resource group represents a different area of the Azure environment.

| Resource Group | Purpose |
|---|---|
| `rg-compute` | Compute-related resources and permissions |
| `rg-network` | Network-related resources and permissions |
| `rg-storage` | Storage-related resources and permissions |

---

# Creating the Resource Groups

The resource groups were created through the Azure Portal.

The general process was:

```text
Azure Portal
    ↓
Resource Groups
    ↓
Create
    ↓
Select Subscription
    ↓
Enter Resource Group Name
    ↓
Select Region
    ↓
Review + Create
```

The three resource groups were created separately so that different permissions could later be applied to each environment.

---

# Resource Group Structure

The resulting Azure environment was:

```text
Azure Subscription
│
├── rg-compute
│
├── rg-network
│
└── rg-storage
```

These resource groups later became the scopes used when configuring Azure RBAC.

For example:

```text
Network-Admins
      ↓
Network Role
      ↓
rg-network
```

and:

```text
Storage-Admins
      ↓
Storage Role
      ↓
rg-storage
```

---

# Verification

After creating the resource groups, I verified that all three appeared within the Azure Portal.

<img width="1915" height="546" alt="Entra ID Lab Resource Groups Pic" src="https://github.com/user-attachments/assets/d42eef3c-f226-418c-b45a-e3437fb5c7f9" />

The three resource groups were successfully created and ready for the next stage of the lab: configuring Azure RBAC.

---

# Result

The Azure resource-group structure was successfully created.

The environment now consisted of:

```text
Microsoft Entra ID
│
├── Users
└── Security Groups

Azure
│
├── rg-compute
├── rg-network
└── rg-storage
```

The next step was connecting the identities and groups to these Azure resources through Azure RBAC role assignments.
