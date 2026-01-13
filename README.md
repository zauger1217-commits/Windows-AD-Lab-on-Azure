# 🏢 Windows Active Directory Lab on Microsoft Azure

This project is a **step-by-step lab walkthrough** for deploying a **Windows Active Directory environment** in **Microsoft Azure**.  
The lab simulates a real-world enterprise setup by deploying **two domain controllers** within the same domain for redundancy and replication.

This solution is ideal for learners whose local machines cannot handle virtualization, as all infrastructure runs in the cloud and is accessed via **HTTPS and RDP**.

---

## 📘 Overview

This lab walks through setting up an **Active Directory home lab** using Azure infrastructure.  
Once complete, you will have a functional domain with **dual domain controllers**, DNS configuration, and replication validation.

### Recommended Practice After Completion
- Create and delete user accounts
- Reset passwords
- Configure permissions and group policies
- Explore DNS, DHCP, and domain controller roles
- Simulate real-world enterprise directory structures

---

## 🎯 Learning Objectives

- Deploy Azure networking and compute resources
- Install and configure Active Directory Domain Services (AD DS)
- Promote and replicate domain controllers
- Configure DNS for Active Directory environments
- Validate multi-DC replication

---

## ✅ Prerequisites

- **Microsoft Azure account**
- **Windows Remote Desktop Connection (RDP)**
- Basic familiarity with Windows Server

---

## 🚀 Lab Steps

## 1️⃣ Create a Resource Group

1. Log in to the **Azure Portal**
2. Go to **Home → Resource groups**
3. Click **Create**
4. Name the resource group: `ADLAB`

---

## 2️⃣ Create a Virtual Network

- **Name:** `OnSite`

### Configuration
- **Subnet address range:** `10.0.1.0/24`
- **Security settings:** Disable all options
- Skip **Tags**
- Click **Review + Create**

---

## 3️⃣ Create First Domain Controller (DC1)

1. Navigate to **Virtual Machines → Create**
2. Add to resource group: `ADLAB`
3. **VM Name:** `DC1`
4. **Availability Option:** Availability set  
   - Create availability set named `ADLAB`
5. **Image:** Windows Server 2019 (Or 2022)
6. **Size:** 2 vCPUs / 8 GB RAM
7. Create local admin username and password

### Disk Configuration
- OS Disk: **Standard HDD**
- Attach an additional **10 GB disk** for Active Directory

### Networking
- Virtual Network: `OnSite`
- RDP Port: **3389**
- In Monitoring tab-Disable **Boot Diagnostics**

Deploy the VM.

---

## 4️⃣ Configure DC1

1. Connect to `DC1` via **RDP**
2. Format the **F:** drive (AD storage disk)
3. look in disk management- using extra 10GB volume
4. Open **Server Manager**
5. Install **Active Directory Domain Services**
6. Promote server to a domain controller (last step in installing Domain Services)
7. Create a new forest:
   ```
   myazurelab.com
   ```
8. Store AD data on the **F:** drive (first option only)
9. Restart the VM

---

## 5️⃣ Create Second Domain Controller (DC2)

- Repeat Step 3
- **VM Name:** `DC2`
- Same **Resource Group**, **Availability Set**, and **Network**
- Make sure to disable Boot Diagnostics in Monitoring tab
- Attach a **10 GB disk**
- Deploy the VM

---

## 6️⃣ Configure DNS on DC1

1. Set **DC1 IP address** to **Static** (Find Subnetmask and Gateway in cmd with ipconfig)
2. Can leave DNS as loopback address (127.0.0.1) and leave Secondary blank
3. Copy DC1’s private IP address
4. Navigate to the **OnSite Virtual Network**
5. Set **Custom DNS servers**
6. Paste DC1’s IP address
7. Restart **DC1 and DC2**

---

## 7️⃣ Add DC2 to the Domain

1. RDP into **DC2**
2. Format the **F:** drive
3. Join the domain:
   - `myazurelab.com`
   - Local server
   - Workgroup
   - Join domain
4. Authenticate using **DC1 credentials**
5. Restart DC2
6. Install **Active Directory Domain Services**
7. Promote DC2 as a domain controller
8. Replicate from **DC1**

---

## 8️⃣ Configure DNS on DC2

1. Set DC2 IP address to **Static** (Find subnetmask and gateway in cmd using ipconfig)
2. set DNS to IP of DC1
3. Copy DC2’s IP address
4. In the **OnSite Virtual Network**, add DC2 as a **secondary DNS server**
5. Restart **DC1 and DC2**

---

## 9️⃣ Configure Subnets in Active Directory

1. Copy IPv4 subnet from **OnSite VNet**
2. On DC1, open **Active Directory Sites and Services**
3. Add the subnet
4. Assign it to the **OnSite** site

---

## 🔟 Confirm Dual Domain Controllers

1. On **DC1**, create a test user using **Active Directory Users and Computers**
2. Log into **DC2**
3. Verify the test user exists

✅ Replication confirmed — dual domain controllers are operational.

---

## 🧹 Clean-Up (Recommended for Labs)

> ⚠️ Delete resources when finished to avoid charges

1. Go to **Azure Portal → Resource Groups**
2. Select `ADLAB`
3. Click **Delete resource group**
4. Confirm deletion

---

## 📘 Notes

- This lab is intended for **learning and skill development**
- Not production-hardened
- For real-world environments, consider:
  - Azure Bastion
  - Backup strategies
  - Monitoring and logging
  - Security baselines

---

## 🧾 License

This documentation is provided for **educational purposes**.  
Free to use and modify.
