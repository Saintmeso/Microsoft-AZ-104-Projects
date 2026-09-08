# AZ-104 Lab 02b – Manage Governance via Azure Policy

Hands-on Microsoft Azure administration lab focused on **Azure resource tagging, Azure Policy, policy remediation, and resource locks**.

This lab demonstrates how Azure policies can enforce organizational standards, how resource tags can improve resource management and reporting, how existing resources can be brought into compliance through remediation, and how resource locks can protect resources from accidental deletion.

---

## Objectives

- Create and assign tags through the Azure portal
- Use Azure Policy to enforce resource tagging
- Test Azure Policy enforcement
- Use Azure Policy to automatically apply tags to resources
- Configure Azure Policy remediation
- Create and test Azure Resource Locks
- Understand the difference between Azure Policy and Resource Locks
- Understand how Azure governance can be applied to Azure resources

---

##  Scenario

An organization's Azure cloud footprint has grown considerably over the last year.

During a recent audit, a substantial number of resources were discovered without a defined owner, project, or cost center.

To improve the management and governance of Azure resources, the organization wants to:

- Apply resource tags to attach important metadata to Azure resources
- Enforce the use of resource tags for new resources by using Azure Policy
- Update existing resources with resource tags
- Use resource locks to protect configured resources

---

##  Technologies Used

- Microsoft Azure
- Azure Portal
- Azure Resource Groups
- Azure Resource Tags
- Azure Policy
- Azure Policy Assignments
- Azure Policy Remediation
- Azure Storage Accounts
- Azure Resource Locks

---

# Task 1 – Assign Tags via the Azure Portal

In this task, a Resource Group was created and assigned a tag identifying its Cost Center.

Tags are key-value pairs that can be used to identify resource owners, sunset dates, group contacts, cost centers, and other organizational information.

---

## Step 1 – Sign in to the Azure Portal

Sign in to the **Azure portal**.

```text
https://portal.azure.com
```

---

## Step 2 – Open Resource Groups

In the Azure Portal, search for:

```text
Resource groups
```

Select **Resource groups** from the search results.

---

## Step 3 – Create the Resource Group

Select:

**+ Create**

Configure the Resource Group with the following settings:

| Setting | Configuration |
|---|---|
| Subscription | Your Azure subscription |
| Resource Group Name | `az104-rg2` |
| Region | `East US` |

The Resource Group created for this lab was:

```text
az104-rg2
```

> **Note:** A new Resource Group was created for the lab so that all resources could be managed together.

<img width="958" height="809" alt="az-104 policy tagging lab" src="https://github.com/user-attachments/assets/1b94f823-1e99-495e-806f-36b4fa3c2488" />

---

## Step 4 – Configure the Cost Center Tag

Move to the **Tags** tab.

Create the following tag:

| Tag Name | Value |
|---|---|
| `Cost Center` | `000` |

The final tag configuration was:

```text
Cost Center = 000
```

<img width="958" height="809" alt="az-104 policy tagging lab tag creation" src="https://github.com/user-attachments/assets/5f5fa97a-b8ae-431f-b0e1-c6a15dd911b8" />

---

## Step 5 – Review and Create the Resource Group

Select:

**Review + Create**

Review the configuration and select:

**Create**

The Resource Group was successfully created with the Cost Center tag.

---

# Task 2 – Enforce Tagging via Azure Policy

In this task, an Azure Policy was assigned to the Resource Group to require a specific tag and tag value.

The built-in policy used was:

```text
Require a tag and its value on resources
```

The policy was then tested by attempting to create an Azure Storage Account without the required tag.

---

## Step 6 – Open Azure Policy

In the Azure Portal, search for:

```text
Policy
```

Select **Policy**.

---

## Step 7 – Open Policy Definitions

In the **Authoring** section, select:

**Definitions**

This page contains the available built-in and custom Azure Policy definitions.

---

## Step 8 – Search for the Required Tag Policy

Search for:

```text
Require a tag and its value on resources
```

Select the policy definition and review its configuration.

<img width="958" height="807" alt="az-104 policy tagging lag poliy creation" src="https://github.com/user-attachments/assets/08df762c-cd2a-40cd-b1c9-234f462c3a7c" />

---

## Step 9 – Assign the Policy

Select:

**Assign policy**

For the policy scope, select the ellipsis button.

Configure the scope as:

| Setting | Configuration |
|---|---|
| Subscription | Your Azure subscription |
| Resource Group | `az104-rg2` |

Select:

**Select**

The policy was scoped to the Resource Group.

---

## Step 10 – Configure the Policy Assignment

Configure the policy assignment.

Policy enforcement was enabled so that resources failing the policy requirement would not be allowed to deploy.

---

## Step 11 – Configure Policy Parameters

Move to the **Parameters** section.

Configure:

| Setting | Value |
|---|---|
| Tag Name | `Cost Center` |
| Tag Value | `000` |

The policy therefore requires resources to contain:

```text
Cost Center = 000
```

---

## Step 12 – Review the Policy Assignment

Move through the remaining configuration tabs and review the remediation and managed identity settings.

For this policy assignment, a managed identity was not required.

Select:

**Review + Create**

Then select:

**Create**

The policy assignment was successfully created.

> **Note:** Azure Policy assignments may take several minutes to become effective.

---

## Step 13 – Create a Test Storage Account

To test the policy, create a Storage Account inside:

```text
az104-rg2
```

In the Azure Portal, search for:

```text
Storage accounts
```

Select:

**+ Create**

---

## Step 14 – Configure the Test Storage Account

On the **Basics** tab, select:

```text
az104-rg2
```

as the Resource Group.

Enter a globally unique Storage Account name.

The Storage Account name must:

- Be between 3 and 24 characters
- Contain only lowercase letters and numbers
- Be globally unique

---

## Step 15 – Leave the Required Tag Missing

For this test, do not manually add:

```text
Cost Center = 000
```

The purpose of this test is to verify that Azure Policy prevents a resource from being created without the required tag.

---

## Step 16 – Review and Create the Storage Account

Select:

**Review**

Then select:

**Create**

Azure Policy evaluates the deployment.

Because the required Cost Center tag is missing, the deployment is rejected.

A validation error is displayed.

<img width="959" height="810" alt="az-104 policy tagging lab storage account failure validation" src="https://github.com/user-attachments/assets/b151a3e2-c3a0-476e-8eec-b70afe410188" />

This confirmed that the policy successfully enforced the tagging requirement.

The enforcement process was:

```text
Storage Account Deployment
        ↓
Azure Policy Evaluation
        ↓
Cost Center Tag Missing
        ↓
Resource Is Non-Compliant
        ↓
Deployment Denied
```

---

# Task 3 – Apply Tags Automatically with Azure Policy

In this task, the previous required-tag policy assignment was removed and replaced with a policy that automatically inherits a tag from the Resource Group when the tag is missing from a resource.

The policy used was:

```text
Inherit a tag from the resource group if missing
```

---

## Step 17 – Open Policy Assignments

In the Azure Portal, search for:

```text
Policy
```

In the **Authoring** section, select:

**Assignments**

---

## Step 18 – Delete the Previous Policy Assignment

Locate:

```text
Require a tag and its value on resources
```

Select the **ellipsis (...)** next to the policy assignment.

Select:

**Delete assignment**

Confirm the deletion.

The previous policy assignment was removed.


---

## Step 19 – Assign the Tag Inheritance Policy

Select:

**Assign policy**

Configure the scope as:

| Setting | Configuration |
|---|---|
| Subscription | Your Azure subscription |
| Resource Group | `az104-rg2` |

Select:

**Select**

---

## Step 20 – Select the Policy Definition

For **Policy definition**, select the ellipsis button.

Search for:

```text
Inherit a tag from the resource group if missing
```

Select the policy.

<img width="959" height="804" alt="az-104 apply tagging via an Azure Policy" src="https://github.com/user-attachments/assets/e5f8a73e-90bb-440a-ba67-3df4e4879379" />

---

## Step 21 – Configure the Policy Assignment

Select:

**Add**

Configure the policy assignment.

Policy enforcement was enabled.

---

## Step 22 – Configure the Policy Parameter

Move to the **Parameters** section.

Configure:

| Setting | Value |
|---|---|
| Tag Name | `Cost Center` |

The Resource Group already contains:

```text
Cost Center = 000
```

The policy will use the Resource Group's Cost Center tag when a resource does not already contain the tag.

---

## Step 23 – Configure Remediation

Move to the **Remediation** tab.

Enable:

```text
Create a remediation task
```

The remediation task allows Azure Policy to bring existing non-compliant resources into compliance.

---

## Step 24 – Configure the Managed Identity

Because the policy uses the **Modify** effect, a managed identity is required.

Configure a managed identity for the policy assignment.

The managed identity allows Azure Policy to make the required modification to resources.

---

## Step 25 – Review and Create the Policy

Select:

**Review + Create**

Review the configuration.

Then select:

**Create**

The policy assignment was successfully created.

---

## Step 26 – Allow the Policy to Take Effect

Allow several minutes for the policy assignment to become effective.

The policy is now configured to automatically inherit the Resource Group's Cost Center tag when the tag is missing from a resource.

---

## Step 27 – Create Another Storage Account

Create another Storage Account inside:

```text
az104-rg2
```

Navigate to:

**Storage Accounts → + Create**

---

## Step 28 – Configure the New Storage Account

On the **Basics** tab, configure the Resource Group as:

```text
az104-rg2
```

Enter a valid globally unique Storage Account name.

Do not manually add the Cost Center tag.

<img width="961" height="807" alt="az-104 storage account tagging via an azure policy verification" src="https://github.com/user-attachments/assets/3bf51baf-964d-431c-b3b0-6242c7111484" />

---

## Step 29 – Review the Storage Account Deployment

Select:

**Review**

The deployment should pass because the tag inheritance policy is configured to automatically apply the required tag.

---

## Step 30 – Create the Storage Account

Select:

**Create**

The Storage Account was successfully deployed.

<img width="961" height="807" alt="az-104 storage account tagging via an azure policy verification" src="https://github.com/user-attachments/assets/3bfbedaa-74bc-4fa1-918d-5d4f93e4993a" />

---

## Step 31 – Verify the Automatically Applied Tag

Select:

**Go to resource**

Navigate to:

**Tags**

Verify that the Storage Account contains:

```text
Cost Center = 000
```

The Cost Center tag was automatically inherited from the Resource Group.

<img width="962" height="869" alt="az-104 storage account tagging via an azure policy verification pic of tagging" src="https://github.com/user-attachments/assets/3b386c7c-7904-4ea9-965f-c18158702178" />

The process was:

```text
Resource Group
Cost Center = 000
        ↓
Azure Policy
        ↓
Resource Missing Cost Center Tag
        ↓
Policy Modify / Remediation
        ↓
Cost Center = 000
        ↓
Resource Compliant
```

---

# Task 4 – Configure and Test a Resource Lock

In this task, a Resource Lock was created on the Resource Group to prevent accidental deletion.

Azure provides two primary types of resource locks:

- **Delete**
- **Read-only**

A **Delete** lock was configured for this lab.

---

## Step 32 – Open the Resource Group

In the Azure Portal, search for:

```text
Resource groups
```

Open:

```text
az104-rg2
```

---

## Step 33 – Open Resource Locks

Inside the Resource Group, navigate to:

**Settings → Locks**

---

## Step 34 – Add a Resource Lock

Select:

**Add**

Configure the lock as:

| Setting | Configuration |
|---|---|
| Lock Name | `rg-lock` |
| Lock Type | `Delete` |

The final configuration was:

```text
Lock Name = rg-lock
Lock Type = Delete
```

<img width="960" height="866" alt="az-104 rg locks deletion setup" src="https://github.com/user-attachments/assets/b44815f3-38dd-4a70-821f-6151e0b0b433" />

Select:

**OK**

---

## Step 35 – Verify the Resource Lock

Review the Resource Group's **Locks** section.

Verify that the following lock exists:

```text
rg-lock
```

with the lock type:

```text
Delete
```

The Resource Group is now protected from accidental deletion.

---

## Step 36 – Attempt to Delete the Resource Group

Navigate back to:

```text
az104-rg2
```

Open the **Overview** blade.

Select:

**Delete resource group**

---

## Step 37 – Confirm the Resource Group Name

Azure requires the Resource Group name to be entered before deletion.

Enter:

```text
az104-rg2
```

into the confirmation field.

---

## Step 38 – Attempt the Deletion

Select:

**Delete**

Confirm the deletion when prompted.

Azure evaluates the Resource Lock before completing the deletion.

---

## Step 39 – Verify the Deletion Was Blocked

The deletion attempt was denied because the Resource Group contains a Delete Lock.

Azure displayed a failure notification indicating that the Resource Group could not be deleted while the lock existed.

<img width="966" height="910" alt="az-104 rg locks deletion confirmation" src="https://github.com/user-attachments/assets/20e30b1f-2cd3-4d8f-8ebe-5e6db82f0238" />

The process was:

```text
Delete Resource Group
        ↓
Azure Checks Resource Locks
        ↓
Delete Lock Detected
        ↓
Deletion Blocked
```

This confirmed that the Resource Lock was functioning correctly.

---

# 🔐 Azure Governance Concepts

## Resource Tags

Azure Tags are key-value pairs used to provide metadata about Azure resources.

The tag used in this lab was:

```text
Cost Center = 000
```

Tags can be used for:

- Cost tracking
- Resource organization
- Ownership
- Project identification
- Department identification
- Reporting
- Governance

---

## Azure Policy

Azure Policy allows administrators to establish and enforce organizational standards.

The first policy used in this lab was:

```text
Require a tag and its value on resources
```

This policy prevented a Storage Account from being deployed without:

```text
Cost Center = 000
```

---

## Policy Inheritance

The second policy used was:

```text
Inherit a tag from the resource group if missing
```

This policy allowed the Cost Center tag from the Resource Group to be automatically applied to resources that were missing the tag.

---

## Policy Remediation

Policy remediation allows Azure Policy to modify existing resources that are not compliant with a policy.

The remediation process used a managed identity because the policy uses the **Modify** effect.

---

## Resource Locks

Resource Locks provide an additional layer of protection for Azure resources.

The Resource Lock created in this lab was:

```text
Lock Name = rg-lock
Lock Type = Delete
```

The lock successfully prevented the Resource Group from being deleted.

---

#  Testing & Validation

| Test | Expected Result | Result |
|---|---|---|
| Create Resource Group | `az104-rg2` created | ✅ Passed |
| Add Cost Center tag | `Cost Center = 000` | ✅ Passed |
| Assign required-tag policy | Policy assigned | ✅ Passed |
| Deploy resource without Cost Center tag | Deployment denied | ✅ Passed |
| Remove required-tag policy | Assignment removed | ✅ Passed |
| Assign tag inheritance policy | Policy assigned | ✅ Passed |
| Configure remediation | Remediation configured | ✅ Passed |
| Create resource without manually adding tag | Resource created | ✅ Passed |
| Verify Cost Center tag | `Cost Center = 000` automatically applied | ✅ Passed |
| Create Resource Lock | `rg-lock` created | ✅ Passed |
| Attempt Resource Group deletion | Deletion blocked | ✅ Passed |

---

#  Final Environment

```text
Azure Subscription
│
└── az104-rg2
    │
    ├── Tags
    │   └── Cost Center = 000
    │
    ├── Azure Policy
    │   └── Inherit a tag from the resource group if missing
    │       │
    │       ├── Tag = Cost Center
    │       ├── Remediation Enabled
    │       └── Managed Identity
    │
    ├── Storage Account
    │   └── Cost Center = 000
    │
    └── Resource Lock
        └── rg-lock
            └── Delete
```

---

#  Administrative Decisions

## Resource Group-Level Policy Scope

The Azure Policies were scoped specifically to:

```text
az104-rg2
```

This limited the policy's effect to the resources used for the lab rather than affecting unrelated resources in the subscription.

Azure Policies can be assigned at different scopes, including:

```text
Management Group
Subscription
Resource Group
Resource
```

---

## Preventative Governance

The first policy demonstrated preventative governance.

The policy:

```text
Require a tag and its value on resources
```

prevented the deployment of a resource that did not contain:

```text
Cost Center = 000
```

---

## Remediating Governance

The second policy demonstrated remediating governance.

The policy:

```text
Inherit a tag from the resource group if missing
```

automatically applied:

```text
Cost Center = 000
```

to a resource when the tag was missing.

---

## Resource Protection

The Resource Lock provided an additional layer of protection.

The:

```text
rg-lock
```

Delete Lock prevented the Resource Group from being accidentally deleted.

---

# Cleanup

Before deleting the Resource Group, the Resource Lock must first be removed.

Navigate to:

```text
az104-rg2
→ Settings
→ Locks
```

Delete:

```text
rg-lock
```

After removing the lock, the Resource Group can be deleted.

Navigate to:

```text
az104-rg2
→ Overview
→ Delete resource group
```

Enter:

```text
az104-rg2
```

and confirm the deletion.

---

#  AZ-104 Skills Demonstrated

- Azure Resource Groups
- Azure Resource Tags
- Azure Governance
- Azure Policy
- Azure Policy Assignments
- Azure Policy Parameters
- Azure Policy Enforcement
- Azure Policy Remediation
- Azure Policy Modify Effect
- Managed Identities
- Azure Resource Locks
- Azure Storage Accounts
- Resource Compliance
- Resource Protection
- Azure Portal Administration
- Azure Deployment Validation
