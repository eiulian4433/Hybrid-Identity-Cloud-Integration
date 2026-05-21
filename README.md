# My Home Lab: Active Directory Setup & Hybrid Cloud Sync

## 📋 Project Overview
I built an isolated enterprise network from scratch to practice the exact day-to-day tasks of an IT Support Specialist. I set up local network services, created a structured corporate directory, locked down user machines with security rules, configured secure file sharing, and linked everything up to the cloud using Microsoft Entra ID.

---

## 🛠️ Step 1: Setting Up the Virtual Lab & Network
- **The Machines:** Deployed a 5-VM environment using **Windows Server 2022** and **Windows 10 Pro** clients.
  <img width="805" height="868" alt="Screenshot (28)" src="https://github.com/user-attachments/assets/e2c78131-12fa-4c73-bbb4-9b16e08dcf61" />

- **Static IPs:** Assigned fixed IP addresses to my core servers so they never lose communication:
  - Domain Controller (`DC-01`): `192.168.1.5`
    <img width="1389" height="1119" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/003fd7a4-58f9-42bf-944a-fa083bc653b7" />

  - Backup/Second Server (`SV-02`): `192.168.1.6`
    <img width="1585" height="1210" alt="Screenshot (37)" src="https://github.com/user-attachments/assets/3bcf7bbc-274a-4708-a24b-fd37906a8024" />

- **Routing & Internet:** Set up **NAT and RRAS** (Routing and Remote Access) so my internal lab machines can safely download updates from the internet without being exposed to the outside world.
 <img width="1381" height="1147" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/593f662b-cc0b-4aac-b615-5dfffe565a7f" />

- **DHCP & DNS:** Configured a DHCP scope (`192.168.1.100` to `.200`) to automatically give IP addresses to my client machines, setting the gateway to `192.168.1.1` and DNS to my server (`192.168.1.5`).
<img width="1385" height="1142" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/a9d7eea9-9e97-4278-82c2-dd52c5e8d664" />
<img width="1379" height="1142" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/cda19fd8-1664-4577-8970-6f68cb959825" />

- **Joining the Domain:** Created a local domain called `lab.local` and joined all 3 Windows 10 client computers to it.
<img width="1389" height="1142" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/4d36f484-e045-46e2-8e38-0df0eb00d264" />
---

## 👥 Step 2: Active Directory & Managing Users

<img width="1391" height="1153" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/0ab1c5b4-056d-4747-a957-e6661ca407ba" />

- **Admin Account:** Created a separate domain admin account for myself to follow best practices (instead of using the default Administrator account).
 <img width="1388" height="1144" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/b2217d5c-1770-4538-bfea-ebebd4ff5dfe" />

- **Organization Structure:** Built a main Organizational Unit (OU) called `Europe`. Inside it, I made sub-folders for different departments like *IT, Design, Administration, Finance, Sales, HR, Marketing, Development, and Customer Service*.
- **Creating Users:** Manually created 15 different test users and placed them into their correct department folders to simulate a real company workforce.

---

## 🔐 Step 3: Security & Group Policy Objects (GPOs)
I created and tested Group Policies to see how an IT department locks down company computers:
- **Password Rules:** Enforced a strong password policy (minimum 10 characters, locked out after 2 failed attempts, with a 30-minute cooldown).
- **USB Block:** Blocked all access to USBs and removable storage to prevent malware or data theft.
- **App/Driver Lock:** Disabled standard users from installing hardware drivers.
- **Settings Block:** Completely blocked access to the Control Panel and Windows Settings.
- **Testing My Work:** Logged into a client machine as a standard user and verified that the Control Panel was blocked. I also tried to change a password to a short one and watched the system block it, proving my security rules work!

---

## 📁 Step 4: Shared Folders & Network Drives
- **Security Groups:** Created specific security groups for each department (`SG_it`, `SG_design`, `SG_administration`, etc.) and added my 15 users to them.
- **Secure File Share:** Created an `it_department` folder and secured it so only the IT team could see it:
  - Removed the default "Everyone" and "Authenticated Users" access.
  - Disabled folder inheritance so permissions don't bleed down from the C: drive.
  - Gave `SG_it` explicit "Modify" permissions.
- **Testing Permissions:** Logged in as an IT user and confirmed they could access the folder. Then, logged in as an HR user and confirmed they were blocked.
- **Mapped Network Drives:** Configured a Group Policy to automatically map a `G:` drive on user computers, but used **Item-Level Targeting** so the drive only appears for people inside the `SG_it` security group.

---

## ☁️ Step 5: Connecting to the Hybrid Cloud (Entra ID)
- **Microsoft Entra Connect:** Downloaded and ran the setup wizard to sync my local lab with the cloud.
- **Filtering Users:** Adjusted the sync settings so it only uploaded my employee folders, leaving behind local system accounts.
- **Cloud Verification:** Logged into the cloud-based **Microsoft Entra Admin Center** and verified that all 15 users were successfully created online and marked as "On-premises synced."
