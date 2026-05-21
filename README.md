# My Home Lab: Active Directory Setup & Hybrid Cloud Sync

## 📋 Project Overview
I built an isolated enterprise network from scratch to practice the exact day-to-day tasks of an IT Support Specialist. I set up local network services, created a structured corporate directory, locked down user machines with security rules, configured secure file sharing, and linked everything up to the cloud using Microsoft Entra ID.

---

## 🛠️ Step 1: Setting Up the Virtual Lab & Network
- **The Machines:** Deployed a 5-VM environment using **Windows Server 2022** and **Windows 10 Pro** clients.
- **Static IPs:** Assigned fixed IP addresses to my core servers so they never lose communication:
  - Domain Controller (`DC-01`): `192.168.1.5`
  - Backup/Second Server (`SV-02`): `192.168.1.6`
- **Routing & Internet:** Set up **NAT and RRAS** (Routing and Remote Access) so my internal lab machines can safely download updates from the internet without being exposed to the outside world.
- **DHCP & DNS:** Configured a DHCP scope (`192.168.1.100` to `.200`) to automatically give IP addresses to my client machines, setting the gateway to `192.168.1.1` and DNS to my server (`192.168.1.5`).
- **Joining the Domain:** Created a local domain called `lab.local` and joined all 3 Windows 10 client computers to it.

---

## 👥 Step 2: Active Directory & Managing Users
- **Admin Account:** Created a separate domain admin account for myself to follow best practices (instead of using the default Administrator account).
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
