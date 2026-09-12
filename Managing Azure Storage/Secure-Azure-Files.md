# 03 – Secure Azure Files & Network Access

## Objective

Create an **Azure File Share**, upload data using **Storage Browser**, and restrict the storage account to an **Azure Virtual Network**.

---

## 1. Create an Azure File Share

Created a classic file share named `share1`.

- **File Share:** `share1`
- **Access Tier:** Transaction Optimized
- **Protocol:** SMB
- **Backup:** Disabled

<img width="958" height="606" alt="az-104 manage azure storage file share configuration" src="https://github.com/user-attachments/assets/93ff87fb-f510-4d2d-97bb-a4d03f3cf9ef" />

---

## 2. Upload Data Using Storage Browser

Opened **Storage Browser** and navigated to `share1`. A file was successfully uploaded to the file share.

<img width="964" height="663" alt="az-104 manage azure storage file share upload pic" src="https://github.com/user-attachments/assets/03301f1b-3901-468d-b7fa-a7c835b8a3a7" />

---

## 3. Create the Virtual Network

Created an Azure Virtual Network named `vnet1` in the `az104-rg7` resource group.

<img width="958" height="734" alt="az-104 manage azure storage vnet creation" src="https://github.com/user-attachments/assets/7bdec238-19fe-4a88-bca3-10055a9650b3" />

The virtual network was successfully deployed.

---

## 4. Configure the Storage Service Endpoint

Added the **Microsoft.Storage** service endpoint to the `default` subnet of `vnet1`.

<img width="960" height="693" alt="az-104 manage azure storage service endpoints creation" src="https://github.com/user-attachments/assets/c7a083f3-e9cb-4033-8cae-3d296bbb82e7" />

---

## 5. Restrict Storage Access to the VNet

Added `vnet1` and its `default` subnet to the storage account's allowed virtual networks.

The storage account was configured to restrict access to approved network sources.

<img width="957" height="788" alt="az-104 manage azure storage ensuring network via vnet" src="https://github.com/user-attachments/assets/5378f9d1-39a4-4875-b50f-872ecab06e5b" />

---

## 6. Troubleshooting Network Access

After restricting the storage account to `vnet1`, attempting to access the blob container from the public network resulted in:

```text
This request is not authorized to perform this operation.
Error code: 403
```

<img width="956" height="769" alt="az-104 manage azure storage vnet confirmation pic" src="https://github.com/user-attachments/assets/0bd56181-236f-4277-98c0-963e035754ef" />

### Cause

The storage account's public network access was restricted, but my current **IPv4 address was not included in the allowed network rules**.

### Resolution

I returned to:

**Storage Account → Security + networking → Networking**

and added my IPv4 address to the allowed addresses. After saving the configuration, I returned to the `data` blob container and access was restored.

This demonstrated that **Azure Storage access requires both the correct identity permissions and an approved network path**.

---

## Result

Successfully created an Azure File Share and secured the storage environment using:

```text
Azure File Share
      │
      └── Storage Browser
              │
              ▼
          Storage Account
              │
       Network Restrictions
              │
            vnet1
              │
      Microsoft.Storage
       Service Endpoint
```
