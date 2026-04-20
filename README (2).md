# 🖥️ Windows Server 2016 — Network Services Lab

> **Lab 6 · NIT2122 Lab Assessment 2** | Managing Windows Server Network Services  
> Hands-on installation and configuration of **DHCP** and **IIS** roles on Windows Server 2016 via Microsoft Azure VM

---

<div align="center">

![Windows Server 2016](https://img.shields.io/badge/Windows%20Server-2016-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![DHCP](https://img.shields.io/badge/Role-DHCP%20Server-00B4D8?style=for-the-badge&logo=microsoft&logoColor=white)
![IIS](https://img.shields.io/badge/Role-IIS%20Web%20Server-5CB85C?style=for-the-badge&logo=microsoft&logoColor=white)
![Azure VM](https://img.shields.io/badge/Platform-Azure%20VM-0089D6?style=for-the-badge&logo=microsoftazure&logoColor=white)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Lab Objectives](#-lab-objectives)
- [Prerequisites](#-prerequisites)
- [Activity 7-1 · Installing and Configuring DHCP](#-activity-7-1--installing-and-configuring-dhcp)
  - [Step 1.0 — Server Verification (date + ipconfig)](#step-10--server-verification-date--ipconfig)
  - [Step 1.10 — DHCP Role Features Wizard](#step-110--dhcp-role-features-wizard)
  - [Step 1.14 — DHCP Management Console](#step-114--dhcp-management-console)
  - [Step 1.17 — New Scope Name Configuration](#step-117--new-scope-name-configuration)
  - [Step 1.18 — IP Address Range Setup](#step-118--ip-address-range-setup)
  - [Step 1.35 — DNS Dynamic Update Settings](#step-135--dns-dynamic-update-settings)
  - [DHCP Q&A](#-dhcp-qa)
- [Activity 7-2 · Installing and Configuring IIS](#-activity-7-2--installing-and-configuring-iis)
  - [Step 2.0 — Server Verification (date + ipconfig)](#step-20--server-verification-date--ipconfig)
  - [Step 2.6 — Web Server (IIS) Role Features Wizard](#step-26--web-server-iis-role-features-wizard)
  - [Step 2.12 — IIS Client Certificate Mapping Authentication](#step-212--iis-client-certificate-mapping-authentication)
  - [Step 2.18 — Virtual Directory Creation](#step-218--virtual-directory-creation)
  - [Step 2.27 — Default Document Configuration](#step-227--default-document-configuration)
  - [Step 2.33 — IIS User Security Permissions](#step-233--iis-user-security-permissions)
  - [Step 2.38 — Connection Limits Configuration](#step-238--connection-limits-configuration)
  - [IIS Q&A](#-iis-qa)
- [Key Takeaways](#-key-takeaways)

---

## 🌐 Overview

This lab provides hands-on experience with two critical **Windows Server 2016** network service roles, configured through **Server Manager** on a **Microsoft Azure Virtual Machine**:

| Service | Role | Purpose |
|---------|------|---------|
| **DHCP** | Dynamic Host Configuration Protocol | Automatically assigns IP addresses and network configuration to clients |
| **IIS** | Internet Information Services | Hosts web applications and static websites on Windows Server |

---

## 🎯 Lab Objectives

- ✅ Install the **DHCP Server** role using the Add Roles and Features Wizard
- ✅ Create and configure a **DHCP Scope** with IP range, exclusions, and lease duration
- ✅ Enable **DNS Dynamic Updates** from the DHCP server
- ✅ Install the **Web Server (IIS)** role with security modules including Client Certificate Mapping
- ✅ Create a **Virtual Directory** under the Default Web Site in IIS Manager
- ✅ Configure **connection limits**, **default documents**, and **directory browsing** in IIS
- ✅ Review and apply **user security permissions** for anonymous web clients

---

## ⚙️ Prerequisites

- Windows Server 2016 running on a **Microsoft Azure VM**
- Administrator credentials with domain admin rights
- DNS server address obtained from Activity 3-1 (or local machine IP)
- Known **subnet mask** and **DHCP authorization** details
- Active **Server Manager** session

---

## 🔵 Activity 7-1 · Installing and Configuring DHCP

**Objective:** Install the DHCP Server role and configure a DHCP scope with automatic IP address assignment, exclusions, and DNS dynamic update integration.

**Description:** DHCP is installed as a server role in Windows Server 2016 using Server Manager. You install DHCP, configure an address scope, set exclusions and lease durations, then verify DNS dynamic update settings are correctly applied.

> 📸 **Screenshots required at steps:** `1.0` · `1.10` · `1.14` · `1.17` · `1.18` · `1.35`  
> 💬 **Questions answered at steps:** `1.19` · `1.21` · `1.25` · `1.28`

---

### Step 1.0 — Server Verification (date + ipconfig)

Open a **Command Prompt** or **PowerShell** window and run both commands on one line to confirm the server identity, current date/time, and full network configuration on the Azure VM:

```powershell
date; ipconfig /all
```

Capture the screen with the **date and time visible in the Azure VM taskbar**.

![figure-1.png](figure-1.png)

![figure-2.png](figure-2.png)

> 📌 *The output confirms the VM hostname (`WIN-KJ6L3H2K9R7`), IPv4/IPv6 addresses, subnet mask, default gateway, DNS servers, and MAC address. Timestamping the environment at the start of each activity creates an auditable record of the lab state — required for assessment submission and reproducibility.*

---

### Step 1.10 — DHCP Role Features Wizard

In the **Add Roles and Features Wizard**:

1. In the *Select server roles* window, check the box for **DHCP Server**
2. A popup appears asking to add required features — click **Add Features** to include **DHCP Server Tools** (the management console)
3. Click **Next** through *Select server roles* and *Select features*
4. Read the information in the **DHCP Server** window carefully before clicking **Next**
5. Click **Install** in the Confirm installation selections window, then **Close**

![figure-9.png](figure-9.png)

> 📌 *The "Add features that are required for DHCP Server?" dialog confirms that Remote Server Administration Tools and DHCP Server Tools will be added. DHCP Server Tools provide the graphical management console (available at Tools → DHCP after installation) needed for all scope creation, server authorization, and lease management.*

---

### Step 1.14 — DHCP Management Console

After installation completes, open the DHCP console via **Tools → DHCP**. In the left pane, **double-click the server name** (or click the right-pointing arrow) to expand it. Confirm that both **IPv4** and **IPv6** nodes are visible underneath.

Click **IPv4** in the left pane to view configuration details in the middle pane.

![figure-10.png](figure-10.png)

> 📌 *Both IPv4 and IPv6 nodes appearing under the server confirms the DHCP role installed and is ready for scope configuration. The DHCP management dashboard in Server Manager also surfaces any initial event log warnings — these are expected before the server is authorized in Active Directory and a scope is activated.*

---

### Step 1.17 — New Scope Name Configuration

In the left pane, right-click **IPv4** and click **New Scope**. After the New Scope Wizard starts and you click **Next**, configure the scope identity:

| Field | Value |
|-------|-------|
| **Name** | `AdminYourLastName` (e.g., `AdminSmith`) |
| **Description** | `Admin area subnet` |

Click **Next** to proceed.

![figure-11.png](figure-11.png)

> 📌 *A clear, consistent naming convention is critical in environments with multiple scopes across VLANs and subnets. The `AdminYourLastName` format immediately identifies both the subnet purpose and the responsible administrator — reducing misconfiguration risk during maintenance and audits.*

---

### Step 1.18 — IP Address Range Setup

Configure the IP address pool for this scope. Use the **period key (`.`)** to move between octets in the IP address fields when entering fewer than three digits:

| Setting | Value |
|---------|-------|
| **Start IP Address** | `198.4.100.51` |
| **End IP Address** | `198.4.100.99` |
| **Subnet Mask** | `255.255.255.0` |

Click **Next** to proceed to exclusions.

![figure-12.png](figure-12.png)

> 📌 *This defines a pool of 49 leasable addresses (`.51` through `.99`) within the `198.4.100.0/24` subnet. Reserving the lower range (`.1`–`.50`) for static assignments — servers, printers, and network devices — is standard enterprise practice. The `/24` subnet mask supports up to 254 hosts on this segment.*

---

### Step 1.35 — DNS Dynamic Update Settings

Navigate to **IPv4 → right-click → Properties → DNS tab** and verify the following settings are configured correctly:

| Setting | Status |
|---------|--------|
| Enable DNS dynamic updates according to the settings below | ✅ Checked |
| Dynamically update DNS records only if requested by the DHCP clients | ✅ Selected |
| Discard A and PTR records when lease is deleted | ✅ Checked |

Click **OK** to apply, then close the DHCP window.

![figure-13.png](figure-13.png)

> 📌 *These settings keep DNS records synchronized with active DHCP leases. "Update only if requested" is correct for modern clients (Windows 7/8/10/Server 2012+) that self-register. Enabling "Discard A and PTR records when lease is deleted" is critical — without it, expired lease records accumulate in DNS and cause stale name resolution failures in the network.*

---

## 💬 DHCP Q&A

<details>
<summary><strong>Step 1.19 — What happens after clicking Add in the exclusion window? Is an ending address required?</strong></summary>

> The specified IP address is **immediately added to the exclusion list** displayed below the input fields. An **ending address is not required** — entering only a start address excludes that single IP. To exclude a contiguous range, both a start and end address must be provided.

</details>

<details>
<summary><strong>Step 1.21 — What is the default lease time? For what types of situations is it appropriate?</strong></summary>

> The default lease duration is **8 Days, 0 Hours, 0 Minutes**.  
>
> This is appropriate for **stable, wired office environments** where devices (desktops, servers, network printers) remain connected consistently and IP turnover is low. It reduces DHCP renewal traffic without risking address pool exhaustion. Shorter leases (1–2 hours) suit high-turnover environments such as conference rooms, hotel networks, or public Wi-Fi hotspots.

</details>

<details>
<summary><strong>Step 1.25 — How would you enter more than one DNS server?</strong></summary>

> Enter the IP address of the first DNS server in the **IP address** field and click **Add**. Then type the second DNS server's IP in the same field and click **Add** again. Each DNS server is appended to the list in priority order — the topmost server is queried first, with subsequent entries used as failover.

</details>

<details>
<summary><strong>Step 1.28 — What now appears in the middle pane of the DHCP window after clicking Finish?</strong></summary>

> The **IP address range of the newly created scope** appears in the middle pane. Selecting **Address Pool** in the left pane (under the scope) reveals the full breakdown: **available addresses**, **leased addresses**, and **excluded addresses** — providing a real-time view of pool utilization.

</details>

---

## 🟢 Activity 7-2 · Installing and Configuring IIS

**Objective:** Install the Web Server (IIS) role and configure virtual directories, security modules, and connection management settings.

**Description:** The **Web Server (IIS)** role transforms Windows Server 2016 into a full-featured website hosting platform. You install IIS with specific security modules, create a virtual directory, configure default documents and directory browsing, and apply connection limits to control server load.

> 📸 **Screenshots required at steps:** `2.0` · `2.6` · `2.12` · `2.18` · `2.27` · `2.33` · `2.38`  
> 💬 **Questions answered at steps:** `2.11` · `2.21` · `2.25`

---

### Step 2.0 — Server Verification (date + ipconfig)

Open a **Command Prompt** or **PowerShell** window and run:

```powershell
date; ipconfig /all
```

Capture the full Azure VM screen with the **taskbar date and time visible** to timestamp the start of the IIS activity.

![figure-8.png](figure-8.png)

![figure-14.png](figure-14.png)

> 📌 *This second baseline capture documents the server environment at the start of the IIS activity. The Azure VM title bar confirms the lab instance (`lab-[...].australiasoutheast.cloudapp.azure.com`), and the PowerShell output shows the private IP address (`10.0.0.4`) assigned within the Azure virtual network. Noting the date difference from Activity 7-1 demonstrates each task was completed in a separate session.*

---

### Step 2.6 — Web Server (IIS) Role Features Wizard

In **Server Manager → Manage → Add Roles and Features**:

1. Proceed through Before You Begin → Role-based installation → Destination server
2. In *Select server roles*, check the box for **Web Server (IIS)**
3. The popup appears — click **Add Features** to include **IIS Management Console**
4. Click **Next** through Select features and Review information windows
5. Click **Install**, then **Close**

![figure-15.png](figure-15.png)

> 📌 *The "Add features that are required for Web Server (IIS)?" dialog confirms that IIS Management Console will be installed. The IIS Management Console (`inetmgr`) is the primary GUI tool for all post-installation IIS configuration — including sites, virtual directories, application pools, security, and bindings. Without it, all configuration would require PowerShell or direct XML edits.*

---

### Step 2.12 — IIS Client Certificate Mapping Authentication

In the **Select role services** window, expand the **Security** section and check:

- ☑️ **IIS Client Certificate Mapping Authentication**

Read the description on the right: this module enables **client digital certificates** (digital IDs from a trusted source) to authenticate users — mapping certificate identities directly to Windows user accounts.

Click **Next** in the Select role services window, then **Install** and **Close**.

![figure-3.png](figure-3.png)

> 📌 *Client Certificate Mapping Authentication provides a strong, passwordless security layer for enterprise applications requiring mutual TLS (mTLS). Unlike Basic or Windows Authentication, it uses digital IDs issued by a trusted Certificate Authority. This module is not installed by default and must be explicitly selected — once enabled, it can be configured per-site in IIS Manager under Authentication.*

---

### Step 2.18 — Virtual Directory Creation

Open **Tools → Internet Information Services (IIS) Manager**. In the left pane tree, expand your server → **Sites**. Right-click **Default Web Site** and click **Add Virtual Directory**. Configure:

| Field | Value |
|-------|-------|
| **Alias** | `wwwFirstName` (e.g., `wwwJohn`) |
| **Physical path** | `C:\Users\Student\wwwYourFirstName` |

> 💡 *Click the browse button (`...`) and use **Make New Folder** to create the physical directory on disk if it does not already exist.*

Click **OK**.

![figure-4.png](figure-4.png)

> 📌 *A virtual directory maps a URL alias to any physical folder on the server without exposing the underlying directory structure to clients. Content in `C:\Users\Student\wwwJohn` becomes accessible via `http://yourserver/wwwJohn` — and the physical storage can be reorganized at any time without changing the public-facing URL.*

---

### Step 2.27 — Default Document Configuration

In IIS Manager, with **Default Web Site** selected in the left pane, double-click **Default Document** in the middle pane. Review the ordered list of files IIS will serve when a client browses to the site root without specifying a filename:

| Priority | File | Entry Type |
|----------|------|------------|
| 1 | `Default.htm` | Local |
| 2 | `Default.asp` | Local |
| 3 | `index.htm` | Local |
| 4 | `index.html` | Local |
| 5 | `iisstart.htm` | Local |

Right-click `iisstart.htm` to confirm the **Move Up** option is available to reprioritize it.

![figure-5.png](figure-5.png)

> 📌 *IIS scans this list in order and returns the first matching file found in the web root. If none of these files exist and Directory Browsing is disabled, IIS returns a `403 Forbidden` error to the client. Place your primary landing page at the top of this list. Click the back arrow in the upper-left to return to the Default Web Site home after reviewing.*

---

### Step 2.33 — IIS User Security Permissions

Navigate to **Default Web Site → right-click → Edit Permissions → Security tab**.

In the **Group or user names** list, click **`Users (s1234567\IIS_IUSERS)`** to view the permissions granted to anonymous web visitors in the domain.

![figure-6.png](figure-6.png)

> 📌 *The `IIS_IUSERS` group governs what anonymous web clients can access on the server's file system. Best practice restricts this group to **Read & Execute** on the web root — preventing anonymous users from writing files or traversing outside the web content directory. The Sharing tab (also visible here) allows configuring SMB file sharing for the same folder independently of IIS permissions.*

---

### Step 2.38 — Connection Limits Configuration

In IIS Manager, select **Default Web Site** in the left pane. In the right **Actions** panel, find the **Configure** section and click **Limits**. Enter:

| Setting | Value |
|---------|-------|
| **Connection time-out (in seconds)** | `240` |
| **Limit number of connections** | ✅ Enabled |
| **Maximum simultaneous connections** | `500` |

Click **OK** to apply. Close the Internet Information Services (IIS) Manager window.

![figure-7.png](figure-7.png)

> 📌 *Connection limits are a frontline defense against resource exhaustion. A 240-second timeout reclaims threads from idle or stalled connections automatically. The 500-connection ceiling should be baselined against real traffic — if the server frequently hits this limit and performance degrades, reduce by 50 at a time and monitor. These values should be revisited and tuned as the site grows.*

---

## 💬 IIS Q&A

<details>
<summary><strong>Step 2.11 — What modules are checked by default in the Select role services window?</strong></summary>

The following IIS role services are selected by default during installation:

**Common HTTP Features**
- Static Content
- Default Document
- Directory Browsing
- HTTP Errors

**Health and Diagnostics**
- HTTP Logging

**Performance**
- Static Content Compression

**Security**
- Request Filtering

**Management Tools**
- IIS Management Console

</details>

<details>
<summary><strong>Step 2.21 — How can you set up the virtual directory folder for sharing?</strong></summary>

> Navigate to the folder's **Properties → Sharing tab**, click **Advanced Sharing**, check **Share this folder**, then configure **Permissions** to grant access to the appropriate users or groups. This creates an SMB network share for the same physical folder, independently of the IIS virtual directory mapping — both access methods can coexist.

</details>

<details>
<summary><strong>Step 2.25 — How can you rename the website? How can you restart a stopped website?</strong></summary>

> **Renaming:** Right-click **Default Web Site** in the IIS Manager left-pane tree, select **Rename**, type the new name, and press Enter.  
>
> **Restarting a stopped site:** Right-click the website → **Manage Website** → click **Start** (to start a stopped site) or **Restart** (to restart a running site). These options are also available in the **Actions** panel on the right side of IIS Manager when the site is selected.

</details>

---

## 💡 Key Takeaways

| Concept | Detail |
|---------|--------|
| **DHCP Role Installation** | Installed via Server Manager; requires Active Directory authorization before it will lease addresses to clients |
| **DHCP Scope** | Defines the leasable IP pool, exclusions, lease duration, gateway, and DNS options — all must be configured before activating |
| **DNS Dynamic Updates** | DHCP auto-registers and deregisters client A and PTR records in DNS; enabling "Discard on lease delete" prevents record accumulation |
| **IIS Role Installation** | Modular install — security features like Client Certificate Mapping must be explicitly selected; not included by default |
| **Virtual Directories** | Map clean URL aliases to any physical path; decouples public URL structure from underlying file system organization |
| **IIS Security** | `IIS_IUSERS` governs anonymous access; Client Certificate Mapping adds passwordless mutual TLS authentication |
| **Connection Limits** | Timeout + connection ceiling protect server resources; baseline against real traffic and tune incrementally as needed |

---

<div align="center">

**NIT2122 · Lab Assessment 2 · Lab 6**  
Windows Server 2016 — Network Services Configuration  
*Completed on Microsoft Azure Virtual Machine · australiasoutheast region*

</div>
