# 02 – Secure Azure Blob Storage

## Objective

Configure a secure Blob Storage environment using **private access, immutable storage, RBAC, blob uploads, and User Delegation SAS**.

---

## 1. Create the Blob Container

Inside the `obetastorageacc104` storage account, a container named `data` was created with **private access** and no anonymous access.

<img width="958" height="796" alt="az-104 manage azure storage blob container creation" src="https://github.com/user-attachments/assets/104f5466-6888-424e-a3a5-a065df06bddd" />

---

## 2. Configure Immutable Storage

A **time-based retention policy** was configured on the `data` container.

- **Policy Type:** Time-based retention
- **Retention Period:** 180 days
- **Version-level immutability:** Disabled
- **Protected append writes:** None

This prevents protected blob data from being modified or deleted during the retention period.

<img width="964" height="728" alt="az-104 manage azure storage time-based retention policy" src="https://github.com/user-attachments/assets/11f3073a-468d-4cb7-b710-e745e5501fdd" />

---

## 3. Configure RBAC

Storage permissions were assigned through **Azure Role-Based Access Control (RBAC)**.

### Storage Blob Data Contributor

Assigned the **Storage Blob Data Contributor** role to the user at the storage account scope.

<img width="957" height="663" alt="az-104 manage azure storage IAM Blob data reader configuration" src="https://github.com/user-attachments/assets/6c8f42e9-f67a-4b04-b8f9-c0e5d8fe4b5e" />

### Storage File Data Privileged Contributor

Assigned the **Storage File Data Privileged Contributor** role to support storage file operations.

<img width="955" height="739" alt="az-104 manage azure stroage IAM Blob storage file data contributor role" src="https://github.com/user-attachments/assets/ec44484c-84d9-4bc5-8c63-ed230c69dbc5" />

---

## 4. Upload Blob Data

A file was uploaded into the `data` container under the `securitytest` directory.

<img width="953" height="653" alt="az-104 manage azure storage upload into blob in file" src="https://github.com/user-attachments/assets/b7ccb220-06d8-477b-a48e-b4f9121a5c79" />
The uploaded file was successfully displayed as a **Block Blob** using the **Hot** access tier.

<img width="960" height="639" alt="az-104 manage azure storage file creation for data" src="https://github.com/user-attachments/assets/9fd596ec-753b-4b0d-b842-2ef7b39c9213" />

---

## 5. Troubleshooting Network Access

While working with the blob container, I encountered a **403 – Not Authorized to Perform This Operation** error.

<img width="953" height="791" alt="az-104 manage azure storage blob data file fail" src="https://github.com/user-attachments/assets/6eb00bf0-c4fd-4ce9-8a51-ca0b49800b3d" />

### Cause

The storage account's **public network access had been disabled**, but I had not added my current **IPv4 address** to the allowed network rules.

Because my client was no longer an approved network source, Azure Storage rejected the request.

### Resolution

I returned to:

**Storage Account → Security + networking → Networking**

I added my current IPv4 address to the allowed addresses and saved the configuration.

<img width="957" height="860" alt="az-104 manage azure storage blob network fix" src="https://github.com/user-attachments/assets/fa623c70-4e3f-4d15-81fe-9ba740ef384d" />

<img width="956" height="766" alt="az-104 manage azure storage blob networking fix" src="https://github.com/user-attachments/assets/b5d57889-fe1c-4b17-b132-67f901d2973c" />

I then returned to the `data` blob container and was able to access the storage resources again.

This troubleshooting process demonstrated how **storage network restrictions can prevent otherwise authorized users from accessing Azure Storage**.

---

## 6. Verify Private Blob Access

The blob URL was opened directly without a SAS token.

Azure returned:

```text
PublicAccessNotPermitted
```

This confirmed that anonymous public access was disabled.

<img width="958" height="248" alt="az-104 manage azure storage url conformation " src="https://github.com/user-attachments/assets/c6546221-9921-46e4-9599-b67e7b5ca538" />

---

## 7. Generate a User Delegation SAS

A **User Delegation SAS** was generated to provide temporary access to the blob.

Configuration:

- **Signing Method:** User delegation key
- **Permission:** Read
- **Protocol:** HTTPS only
- **Start:** 09/11/2026
- **Expiry:** 09/13/2026
- **Allowed IP:** Not restricted

<img width="955" height="168" alt="az-104 manage azure storage SAS confirmation url" src="https://github.com/user-attachments/assets/1d9dcc23-c026-44d1-96fe-8c52c6ab759d" />

The generated SAS URL was tested successfully and provided temporary access to the blob without making the container public.

<img width="955" height="168" alt="az-104 manage azure storage SAS confirmation url" src="https://github.com/user-attachments/assets/f711e682-48fb-416d-91cd-6dbf294772fd" />

---

## Result

The Blob Storage environment was successfully secured using:

```text
Private Container
       │
       ├── Immutable Storage
       │      └── 180-Day Retention
       │
       ├── Azure RBAC
       │
       ├── Network Restrictions
       │
       └── User Delegation SAS
              └── Temporary Read Access
```

The network troubleshooting also demonstrated the importance of configuring **both identity permissions and network access rules** when securing Azure Storage.
