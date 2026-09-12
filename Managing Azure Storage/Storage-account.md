# 01 – Create and Configure an Azure Storage Account

## Lab Objective

Create and configure an Azure Storage Account with **secure networking, redundancy, data protection, encryption, and lifecycle management**.

---

## 1. Create the Storage Account

Created the storage account using the following configuration:

- **Resource Group:** `az104-rg7`
- **Region:** East US
- **Storage Account:** `obetastorageacc104`
- **Primary Service:** Azure Blob Storage or Azure Data Lake Storage
- **Performance:** Standard
- **Replication:** Read-access geo-redundant storage (RA-GRS)
- **Access Tier:** Hot
- **Secure Transfer:** Enabled
- **Blob Anonymous Access:** Disabled
- **Storage Account Key Access:** Enabled

<img width="902" height="758" alt="az-104 manage azure storage" src="https://github.com/user-attachments/assets/21b4c275-0a9f-4881-a02a-83f847cc14d4" />

<img width="726" height="773" alt="az-104 manage azure storage setup pic 2" src="https://github.com/user-attachments/assets/671214dd-ce90-4dde-bba6-88dd185abae8" />

---

## 2. Configure Network Access

Public network access was initially configured as **Disabled** to restrict inbound access to the storage account.

The storage account was later configured to allow access from **selected networks** as part of the network security configuration.

<img width="942" height="515" alt="az-104 manage azure storage public network access pic" src="https://github.com/user-attachments/assets/b406028f-6727-43ab-881c-2ae5c5600779" />

---

## 3. Configure Data Protection

The storage account was configured with built-in data protection features including:

- **Blob soft delete:** Enabled
- **Container soft delete:** Enabled
- **File share soft delete:** Enabled
- **7-day retention**
- **Blob versioning:** Enabled

These settings provide recovery options for accidentally deleted or modified storage data.

---

## 4. Configure Lifecycle Management

A lifecycle management policy was configured to automatically transition inactive blobs to lower-cost storage tiers.

### Move to Cool

Blobs that have not been modified for **30 days** are moved to Cool storage.

<img width="950" height="785" alt="az-104 manage azure storage MoveToCool pic" src="https://github.com/user-attachments/assets/24687aa1-45f3-4c27-b00b-88c029012d9a" />

### Move to Cold

An additional lifecycle rule was configured to move blobs to Cold storage after **60 days**.

<img width="947" height="746" alt="az-104 manage azure storage MoveToCold" src="https://github.com/user-attachments/assets/d29d2035-260d-4d93-8dcc-46998f2abbc7" />

### Move to Archive

An additional lifecycle rule was configured to move blobs to Archive storage after **120 days**.

<img width="956" height="669" alt="az-104 manage azure storage MoveToArchive" src="https://github.com/user-attachments/assets/fff7bb0d-9aee-4e78-93e7-e02bfdbbd231" />

This configuration demonstrates how lifecycle management can automatically reduce storage costs as data becomes less frequently accessed.

---

## Result

The Azure Storage Account was successfully created and configured with **RA-GRS redundancy, secure transfer, data protection, restricted network access, and automated lifecycle management**.

```text
Storage Account
      │
      ├── RA-GRS Replication
      ├── Secure Transfer
      ├── Data Protection
      ├── Network Restrictions
      └── Lifecycle Management
             ├── 30 Days → Cool
             ├── 60 Days → Cold
             └── 120 Days → Archive
```
