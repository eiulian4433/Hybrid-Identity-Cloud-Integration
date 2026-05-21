# Enterprise Infrastructure & Hybrid Identity Implementation Lab

## 📋 Technical Overview
This project demonstrates the end-to-end engineering of an isolated enterprise network infrastructure, moving from an on-premises footprint to a hybrid cloud ecosystem. The architecture implements strict role-based access control (RBAC), centralized network provisioning, zero-trust security policies via Group Objects, secure file-sharing topologies, and directory synchronization with Microsoft Entra ID.

---

## 🛠️ Infrastructure Provisioning & Network Topology
- **Hypervisor Management:** Deployed a 5-node virtualized data center topology consisting of **Windows Server 2022** and **Windows 10 Pro** endpoints.
- **Static Addressing Plane:** Configured deterministic static networking for core infrastructure assets:
  - Primary Domain Controller (`DC-01`): `192.168.1.5`
  - Secondary Member Server (`SV-02`): `192.168.1.6`
- **Edge Security & Routing:** Implemented Network Address Translation (NAT) via **Routing and Remote Access Services (RRAS)** to provide controlled, secure external internet access while shielding internal assets from direct exposure.
- **Core Network Dependencies:** Automated internal addressing by configuring a centralized **DHCP Scope** (`192.168.1.100 - .200 /24`) passing the local gateway (`192.168.1.1`) and primary DNS runtime environment (`192.168.1.5`).
- **Domain Centralization:** Established the local root forest `lab.local` and seamlessly integrated all Windows client machines into the active directory runtime.

---

## 👥 Directory Architecture & Identity Lifecycle Management
- **Administrative Isolation:** Provisioned a dedicated, highly privileged Domain Administrator account to strictly decouple operational adjustments from default root assets.
- **Organizational Unit (OU) Hierarchy:** Modeled a corporate geographic division by engineering a root `Europe` OU, containing discrete departmental sub-OUs (*IT, Design, Administration, Finance, Sales, HR, Marketing, Development, Customer Service*).
- **Object Ingestion:** Manually provisioned and staged 15 enterprise user identities across target organizational units to replicate corporate staff onboarding workflows.

---

## 🔐 System Hardening & Group Policy Enforcement
Designed and implemented corporate security baselines using the Group Policy Management Console (GPMC), verifying enforcement at the client layer:
- **Account Security Baseline:** Mandated an enterprise-grade password policy forcing a minimum length of 10 characters, an account lockout threshold of 2 attempts, and a 30-minute lockout duration.
- **Perimeter Control:** Enforced absolute storage hardware restrictions via a "Deny All Access" policy on removable media drives to prevent data exfiltration.
- **Endpoint Hardening:** Prohibited system modification surfaces by disabling end-user driver installations and completely restricting access to the Control Panel and Windows PC Settings.
- **Policy Compliance Verification:** Successfully audited restrictions on client machines; validated the password complexity baseline by attempting an invalid account reset, which was blocked by domain policy rules.

---

## 📁 Access Control & File Services (RBAC)
- **Security Group Strategy:** Created Departmental Security Groups (`SG_Administration`, `SG_Design`, etc.) and structured explicit user mappings to support a scalable Role-Based Access Control model.
- **Secure File Share Topography:** Provisioned an isolated network share (`it_department`) hardened under the principle of least privilege:
  - Stripped default `Everyone` read access permissions.
  - Disabled NTFS inheritance to completely break global directory visibility down the filesystem tree.
  - Assigned explicit `Modify` permissions to `SG_IT` while purging the generic `Authenticated Users` scope.
- **Cross-Identity Share Audit:** Verified explicit isolation; users belonging to `SG_IT` enumerated and modified files successfully, while unauthorized department accounts were denied entry.
- **Automated Storage Mapping:** Engineered a Group Policy Preference (GPP) drive-mapping object (`GPO_Map_IT_Drive`) leveraging **Item-Level Targeting** to dynamically mount the `G:` drive *only* to verified members of `SG_IT`.

---

## ☁️ Hybrid Cloud Synchronization (Entra ID)
- **Directory Ingestion Engine:** Deployed and configured the Microsoft Entra Connect Synchronization framework on the local directory platform.
- **Scope Scoping & Filtering:** Implemented precise OU target filtering during the configuration phase to exclusively synchronize the corporate workforce identities while omitting standard service accounts.
- **Cloud Verification:** Conducted a post-implementation audit via the **Microsoft Entra Admin Center**, verifying that all 15 identities were successfully active in the cloud instance with an official status identifier of **"On-Premises Synced"**.
