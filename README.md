

# 🖥️ Windows Server 2016 – DHCP & IIS Configuration Lab

This project demonstrates the installation and configuration of core network services in **Windows Server 2016**, focusing on:

* **DHCP (Dynamic Host Configuration Protocol)**
* **IIS (Internet Information Services)**

The lab simulates real-world enterprise tasks such as IP address management, DNS integration, and web server deployment using an Azure-based virtual machine.

---

## 🎯 Objectives

* Deploy and configure DHCP server
* Create and manage IP address scopes
* Configure DNS settings for network clients
* Install and configure IIS web server
* Manage website directories, permissions, and limits

---

## 🛠️ Environment

* Windows Server 2016
* Microsoft Azure Virtual Machine
* Server Manager
* PowerShell / Command Prompt

---

## 📸 Lab Walkthrough

---

### 🔹 DHCP Configuration

#### 📌 System Information Check

![System Info](screenshots/figure-1.png)

Displays system date and full network configuration using:

```
date; ipconfig /all
```

---

#### 📌 DHCP Role Installation

![DHCP Install](screenshots/figure-2.png)

Initial setup of the DHCP Server role through Server Manager.

---

#### 📌 DHCP Server Console

![DHCP Console](screenshots/figure-3.png)

Verification of DHCP installation with IPv4 and IPv6 visibility.

---

#### 📌 Scope Configuration

![Scope Setup](screenshots/figure-4.png)

Creation of a DHCP scope to define IP address distribution.

---

#### 📌 IP Address Range

![IP Range](screenshots/figure-5.png)

Configuration of IP range and subnet mask for client allocation.

---

#### 📌 DNS Dynamic Updates

![DNS Settings](screenshots/figure-6.png)

Ensures automatic DNS record updates for DHCP clients.

---

### 🔹 IIS Configuration

#### 📌 System Check Before IIS

![System Info IIS](screenshots/figure-7.png)

Validation of system configuration before IIS installation.

---

#### 📌 IIS Role Selection

![IIS Role](screenshots/figure-8.png)

Selection of Web Server (IIS) role and required features.

---

#### 📌 Security Feature Configuration

![Security Feature](screenshots/figure-9.png)

Enabling IIS Client Certificate Mapping Authentication.

---

#### 📌 Virtual Directory Setup

![Virtual Directory](screenshots/figure-10.png)

Creation of a virtual directory for hosting web content.

---

#### 📌 Default Document Settings

![Default Doc](screenshots/figure-11.png)

Configuration of default web pages for client requests.

---

#### 📌 User Permissions

![Permissions](screenshots/figure-12.png)

Review of IIS user access and security permissions.

---

#### 📌 Connection Limits Configuration

![Limits](screenshots/figure-13.png)

Setting maximum connections and timeout limits to control server load.

---

## 🧠 Key Learnings

* DHCP automates IP allocation in enterprise environments
* DNS integration improves network reliability and name resolution
* IIS enables web hosting and server-side configuration
* Proper configuration enhances performance and security

---

## 💼 Real-World Relevance

This project reflects practical skills used in:

* System Administration
* Network Engineering
* Security Operations (SOC)

It demonstrates the ability to configure and manage critical infrastructure services in a Windows-based environment.

---
Got you — short, clean, and still powerful 👀✨

---

## 🧩 Skills Demonstrated

* Windows Server administration (DHCP & IIS)
* Network configuration (IP addressing, DNS integration)
* Web server setup and management
* Security awareness (DHCP & IIS attack risks)
* Troubleshooting and system analysis
* Technical documentation and GitHub project presentation


## 🚀 Outcome

Successfully configured:

* A working DHCP server with defined IP scope
* DNS-integrated client addressing
* A functional IIS web server with directory access and connection controls

