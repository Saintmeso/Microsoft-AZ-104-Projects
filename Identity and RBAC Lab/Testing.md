# RBAC Permission Testing

After configuring the Azure RBAC assignments, I tested the environment using the individual test accounts.

The purpose of the testing was to verify that the permissions assigned to each group were working as intended and that users were restricted from performing actions outside of their assigned responsibilities.

---

# Testing Method

Each test followed a simple process:

```text
Sign in as Test User
        ↓
Access Azure
        ↓
Attempt or verify an operation
        ↓
Observe the result
        ↓
Determine whether permissions worked as intended
```

---

# Test 1 — Storage Administrator

The first test was performed using the `storageadmin` account.

The Storage Administrator was given permissions related to storage resources, but should not have permissions to manage networking resources.

I attempted to create a Virtual Network inside:

```text
rg-network
```

The VNet configuration initially passed Azure's validation.

However, when the deployment was submitted, the deployment failed.

```text
storageadmin
      ↓
Attempted VNet Creation
      ↓
rg-network
      ↓
Deployment Failed
```

<img width="900" height="796" alt="attempt to make a vnet on storage account" src="https://github.com/user-attachments/assets/2ca06513-24b3-4a01-a7bb-4a836fe30f93" />

### Result

**PASS**

The Storage Administrator was unable to create the VNet because the account did not have the required network permissions.

This confirmed that the RBAC configuration was restricting the account from performing an operation outside of its assigned responsibilities.

---

# Test 2 — Storage Administrator Role Verification

After the failed VNet deployment, I verified the permissions assigned to the Storage Administrator account.

<img width="1910" height="541" alt="rgstorage account role verification" src="https://github.com/user-attachments/assets/699d2254-edda-4a40-9437-30dfbb694fa8" />

The purpose of this test was to confirm that the account had the appropriate permissions for its assigned role while remaining restricted from the network environment.

### Result

**PASS**

The Storage Administrator had the expected access based on its assigned RBAC permissions.

---

# Test 3 — Network Administrator Role Verification

I then signed in using the `networkadmin` account and verified its permissions.

<img width="899" height="804" alt="rgnetwork account role verification" src="https://github.com/user-attachments/assets/479d8e9e-9030-4609-a923-ee55a3dfb7e0" />

The purpose of this test was to confirm that the Network Administrator had the appropriate network-related permissions.

This also helped demonstrate that permissions were being separated between the different resource areas.

### Result

**PASS**

The Network Administrator had the expected network-related access.

---

# Test 4 — Auditor Role Verification

Finally, I tested the `auditor` account.

The Auditor was assigned the Reader role because the account was intended to provide read-only access for reviewing Azure resources.

<img width="896" height="897" alt="rgauditor account role verification" src="https://github.com/user-attachments/assets/69c99513-6918-4c2f-a2e8-d7a5939cc56d" />

The purpose of this test was to verify that the Auditor could view the Azure environment without receiving unnecessary administrative permissions.

### Result

**PASS**

The Auditor had the expected read-only access.

---

# Testing Results

| Test | Account | Test Performed | Result |
|---|---|---|---|
| 1 | `storageadmin` | Attempted to create VNet in `rg-network` | PASS |
| 2 | `storageadmin` | Verified Storage Administrator permissions | PASS |
| 3 | `networkadmin` | Verified Network Administrator permissions | PASS |
| 4 | `auditor` | Verified Auditor read-only permissions | PASS |

---

# Key Observation — Validation vs Authorization

The Storage Administrator VNet test provided an important lesson.

The VNet configuration was able to pass the initial validation stage, but the actual deployment failed.

This demonstrated that:

```text
Validation ≠ Authorization
```

Passing validation does not necessarily mean that the signed-in user has permission to perform the operation.

The actual deployment had to be attempted to confirm whether the user's RBAC permissions allowed the operation.

---

# Final Result

The testing confirmed that the RBAC configuration was working as intended.

The tests demonstrated that:

- Storage permissions were separated from network permissions.
- Network permissions were assigned to the Network Administrator.
- The Storage Administrator could not perform unauthorized network operations.
- The Auditor received read-only access.
- Group-based RBAC assignments were being enforced when users signed in with their respective accounts.

Testing the accounts directly helped verify that the RBAC configuration was not only configured correctly in Azure, but also enforced when users attempted to interact with Azure resources.
