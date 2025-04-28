# Active-Directory  
Creating and Configuring an On-Premises Active Directory Through Azure VMs

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>

This project demonstrates the deployment and configuration of an on-premises Active Directory infrastructure using Azure Virtual Machines.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol (RDP)
- Active Directory Domain Services (AD DS)
- Windows PowerShell

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Overview</h2>

1. Create two Azure Virtual Machines: one Domain Controller (DC-1) and one Client (Client-1).
2. Configure DC-1’s private IP address as static.
3. Set Client-1's DNS server to DC-1's private IP.
4. Install and configure Active Directory Domain Services (AD DS) on DC-1.
5. Create Organizational Units (OUs) and users.
6. Join Client-1 to the domain.
7. Configure Group Policy Object (GPO) for account lockout policies.

---

<h2>Deployment and Configuration Steps</h2>

<h3>Deploy Virtual Machines</h3>

- Create DC-1 (Windows Server 2022) and Client-1 (Windows 10 21H2) in Azure, under the same virtual network.

---

<h3>Configure Domain Controller (DC-1)</h3>

- Set DC-1’s private IP address to static.
- Remote Desktop into DC-1 and disable the firewall (for easier setup).

---

<h3>Prepare Client-1</h3>

- Set Client-1's DNS to point to DC-1's private IP.
- Restart Client-1 and verify network connectivity using `ping`.

---

<h3>Install Active Directory Domain Services (AD DS)</h3>

- Remote into DC-1.
- Open **Server Manager** ➔ Add Roles and Features ➔ Install Active Directory Domain Services.
- After installation, promote the server to a Domain Controller.
  - Create a new forest: `mydomain.com`.
  - Reboot after promotion.

---

<h3>Create Organizational Units and Admin Accounts</h3>

- Open **Active Directory Users and Computers (ADUC)** on DC-1.
- Create three OUs:
  - `_EMPLOYEES`
  - `_ADMINS`
  - `_CLIENTS`
- Create a user `Jane Brown` (username: `jane_admin`) and add her to the **Domain Admins** group.
- Log in as `mydomain\jane_admin` for administrative tasks.

---

<h3>Join Client-1 to the Domain</h3>

- Log into Client-1 with the local admin account.
- Join the machine to `mydomain.com`.
- Restart Client-1.
- Verify the machine appears in **ADUC** under **Computers**.
- Move Client-1 into the `_CLIENTS` OU.

---

<h3>Create Additional Users with PowerShell</h3>

- Open **PowerShell ISE** on DC-1 as Administrator.
- Paste and run a script to create multiple domain users automatically.
- Verify that new users are placed in the `_EMPLOYEES` OU.

---

<h3>Test User Logins</h3>

- Attempt to log into Client-1 using one of the newly created users to verify domain authentication.

---

<h3>Configure Group Policy for Account Lockouts</h3>

- Open **Group Policy Management Console (GPMC)**.
- Edit the **Default Domain Policy**:
  - Set account lockout threshold: **5 invalid attempts**.
  - Set lockout duration: **30 minutes**.
  - Reset account lockout counter after **10 minutes**.
- Apply the new GPO by running `gpupdate /force` on Client-1.
- Test by intentionally mistyping passwords to trigger the lockout.

---

<h2>Conclusion</h2>

This project demonstrates the full setup of a cloud-based Active Directory environment.  
Key achievements include:

- Configuring a Domain Controller and client machine.
- Installing Active Directory Domain Services.
- Structuring users into Organizational Units.
- Automating user creation through PowerShell.
- Applying Group Policy to enforce security policies.

Through hands-on implementation, this lab replicates real-world enterprise practices for managing Active Directory infrastructures.

