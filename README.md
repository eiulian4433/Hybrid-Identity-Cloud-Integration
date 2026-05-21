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
  <img width="1389" height="1117" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/7f961edc-7b8f-477b-bb42-6eeabb599814" />

- **Creating Users:** Manually created 15 different test users and placed them into their correct department folders to simulate a real company workforce.
<img width="1389" height="1121" alt="Screenshot (119)" src="https://github.com/user-attachments/assets/a186df36-7fe8-4b82-bc1e-568e452fb0f5" />

---

## 🔐 Step 3: Security & Group Policy Objects (GPOs)
I created and tested Group Policies to see how an IT department locks down company computers:
- **Password Rules:** Enforced a strong password policy (minimum 10 characters, locked out after 2 failed attempts, with a 30-minute cooldown).
<img width="1371" height="950" alt="Screenshot (74)" src="https://github.com/user-attachments/assets/48c1cbfa-b6a6-433a-a33b-fc9331c778a3" />
<img width="1856" height="1151" alt="Screenshot (75)" src="https://github.com/user-attachments/assets/4c5c2c6b-7297-4632-8232-c301d0b9b807" />
- **USB Block:** Blocked all access to USBs and removable storage to prevent malware or data theft.
 <img width="1238" height="668" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/0ec54d96-f346-4305-9777-ce4357db052f" />

- **App/Driver Lock:** Disabled standard users from installing hardware drivers.
  <img width="1615" height="814" alt="Screenshot (82)" src="https://github.com/user-attachments/assets/dc6f3283-cbed-4000-a424-864f3e0a9cb1" />

- **Settings Block:** Completely blocked access to the Control Panel and Windows Settings.
  <img width="1353" height="805" alt="Screenshot (83)" src="https://github.com/user-attachments/assets/d15ac9c6-9eb0-499e-b299-18e2b4ecffa6" />

- **Testing My Work:** Logged into a client machine as a standard user and verified that the Control Panel was blocked. I also tried to change a password to a short one and watched the system block it, proving my security rules work!
<img width="1939" height="1182" alt="Screenshot (84)" src="https://github.com/user-attachments/assets/fba6983a-d020-4ae2-b131-74d79183b2a1" />
<img width="1334" height="950" alt="Screenshot (91)" src="https://github.com/user-attachments/assets/1c21d0c6-6e97-4b62-83a0-a6447bc906c6" />

---

## 📁 Step 4: Shared Folders & Network Drives

- **Security Groups:** Created specific security groups for each department (`SG_it`, `SG_design`, `SG_administration`, etc.) and added my 15 users to them.
  <img width="1381" height="1135" alt="Screenshot (116)" src="https://github.com/user-attachments/assets/92d10e60-920b-4ba7-ad5c-9be5c68783bd" />
  <img width="1387" height="962" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/159b983a-e845-4302-a80d-22343ce597f4" />
<img width="1278" height="871" alt="Screenshot (121)" src="https://github.com/user-attachments/assets/a3d2981f-9c28-4fa7-9290-e7c5c0d608f9" />


- **Secure File Share:** Created an `it_department` folder and secured it so only the IT team could see it:
<img width="1324" height="789" alt="Screenshot (122)" src="https://github.com/user-attachments/assets/c4832814-f8a9-4b74-b99d-022874385f18" />
  
  - Removed the default "Everyone" and "Authenticated Users" access.
    <img width="1363" height="740" alt="Screenshot (126)" src="https://github.com/user-attachments/assets/de9a39a1-8088-4347-8fd4-a4b6bb7705b8" />

  - Disabled folder inheritance so permissions don't bleed down from the C: drive.
  - Gave `SG_it` explicit "Modify" permissions.
    <img width="1234" height="761" alt="Screenshot (129)" src="https://github.com/user-attachments/assets/2bd33eca-f336-4d69-b1f6-c409e995c025" />

- **Testing Permissions:** Logged in as an IT user and confirmed they could access the folder. Then, logged in as an HR user and confirmed they were blocked.
  <img width="1006" height="827" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/6d3a670d-89e4-4e94-87e0-b9caf5372c90" />
  <img width="992" height="843" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/a2e876b1-066a-4508-bf72-3a8d818f7664" />


- **Mapped Network Drives:** Configured a Group Policy to automatically map a `I:` drive on user computers, but used **Item-Level Targeting** so the drive only appears for people inside the `SG_it` security group.
<img width="1366" height="1033" alt="Screenshot (136)" src="https://github.com/user-attachments/assets/819daeef-6400-46af-8918-6648b6ca3cba" />
<img width="1357" height="1112" alt="Screenshot (140)" src="https://github.com/user-attachments/assets/85e0af27-7bf6-423e-9cdd-7915f7689e2d" />
<img width="1371" height="1128" alt="Screenshot (143)" src="https://github.com/user-attachments/assets/b3cbff4f-675f-46da-893e-81f5ea7a2543" />
<img width="1002" height="832" alt="Screenshot (145)" src="https://github.com/user-attachments/assets/207fcba9-d827-4217-8560-10039429354c" />

---

## ☁️ Step 5: Connecting to the Hybrid Cloud (Entra ID)
<img width="1377" height="1126" alt="Screenshot (149)" src="https://github.com/user-attachments/assets/1c92ca03-cdaf-4d9a-ab29-7168d22f2653" />

- **Microsoft Entra Connect:** Downloaded and ran the setup wizard to sync my local lab with the cloud.
- <img width="1163" height="828" alt="Screenshot (151)" src="https://github.com/user-attachments/assets/b69fa3d9-aa7e-4be4-8459-3602405b293f" />

- **Filtering Users:** Adjusted the sync settings so it only uploaded my employee folders, leaving behind local system accounts.
- <img width="1088" height="756" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/a2340e5a-b372-4f7c-a4b8-a3f46d2f59d6" />

- **Cloud Verification:** Logged into the cloud-based **Microsoft Entra Admin Center** and verified that all 15 users were successfully created online and marked as "On-premises synced."
<img width="1959" height="1206" alt="Screenshot (158)" src="https://github.com/user-attachments/assets/400b3b83-a589-4cb3-af0d-63eea1f75e6d" />
