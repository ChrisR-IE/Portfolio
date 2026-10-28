> **Note:** For the best visual formatting and layout, you can download the original formal Word document version of this report here: [`Botium-Security-Audit.docx`](./Botium-security-audit.docx). This Markdown version is provided for quick, convenient in-browser reading.

---

# INTERNAL SECURITY AUDIT & COMPLIANCE REPORT

**Project / Scenario:** Botium Toys Security Assessment  
**Author / Security Analyst:** Chris R  
**Date:** 23/10/2026  
**Target Scope:** Botium Toys IT Infrastructure, E-Commerce Systems and Assets  

---

## 1. EXECUTIVE SUMMARY

### 1.1 Objective
The purpose of this audit is to evaluate the current security posture of Botium Toys, identify gaps in technical and administrative controls, assess compliance alignment and provide prioritised remediation recommendations to management.

### 1.2 Scope Summary
**Assets Evaluated:** 
* Devices and network-connected devices (Workstations, Laptops, CCTV, end-user devices and peripherals)
* Products for storefront and online sales
* On-site security (Including adjoining warehouse)
* Management of systems, software and services (Accounting, telecommunication, security, inventory management, database)
* Internal and external network (Including Internet access and external access)
* Data retention, recovery and security
* Legacy system management

**Standards Referenced:** 
* GDPR (Data Protection)
* PCI DSS (Payment Security)
* SOC 2 Trust Services Criteria

---

## 2. ASSET INVENTORY & CONTROLS ASSESSMENT

| Asset ID | Asset Description | Asset Type (Hardware/Software/Data) | Expected Controls | Control Status (Implemented / Deficient / Missing) |
| :--- | :--- | :--- | :--- | :--- |
| **AST-01** | Customer PII/SPII & credit card database | Data | Encryption, Least privilege, separation of duties | **Missing** |
| **AST-02** | Internal network & Internet gateway | Hardware / Network | Firewall rules, Network segmentation, IPS/IDS | **Deficient** (Firewall active but missing IDS/IPS systems) |
| **AST-03** | Company data & Backups | Data / software | Disaster recovery, Automated backup | **Missing** (No backups or disaster recovery plans in place) |
| **AST-04** | End-user devices | Hardware | Antivirus software, Password policies | **Implemented** (Deficient password policies exist and no centralised management) |
| **AST-05** | Office, storefront and warehouse | Hardware/Physical | Physical locks, CCTV, Fire Alarms/Sprinklers | **Implemented** (All physical security measures operational) |
| **AST-06** | Legacy End-of-life systems | Hardware/Software | Manual monitoring and maintenance | **Deficient** (No clear or regular schedule or clear intervention methods in place) |

---

## 3. REGULATORY & COMPLIANCE GAP ANALYSIS

### 3.1 GDPR (General Data Protection Regulation) Compliance
* **Requirement:** Protection of EU customer personal data, data minimization, and secure storage.
* **Identified Gap / Finding:** Customer PII/SPII and card information is unencrypted and accessible by all employees.
* **Risk Level:** Critical
* **Remediation Action:** Implement a strong AES-256 bit encryption to ensure customer data is securely stored and introduce separation of duty and least privilege to ensure only authorised employees have access to the stored data.

### 3.2 PCI DSS (Payment Card Industry Data Security Standard) Compliance
* **Requirement:** Secure processing, transmission, and storage of cardholder data.
* **Identified Gap / Finding:** Customer PII/SPII and card information is unencrypted and accessible by all employees.
* **Risk Level:** Critical
* **Remediation Action:** Implement a strong AES-256 bit encryption to ensure customer data is securely stored at rest.

### 3.3 SOC 2 (Security, Availability, Confidentiality)
* **Requirement:** Operational controls, access management, and system availability.
* **Identified Gap / Finding:** User access controls (such as least privilege) have not been implemented, allowing all employees unrestricted access to sensitive customer data. Additionally, there are no backups or disaster recovery plans to guarantee system availability in the event of an outage.
* **Risk Level:** Critical
* **Remediation Action:** Establish formal user access policies implementing the principle of least privilege, protect confidential customer data with encryption, and institute regular automated data backups paired with a documented disaster recovery plan.

---

## 4. PRIORITIZED RECOMMENDATIONS & ACTION PLAN

* **1. High Priority (Immediate Action):**
  * **Finding:** Customer credit card information and personal PII/SPII are stored unencrypted in the local database, and all employees have unrestricted access to these sensitive records.
  * **Action:** Implement AES-256 encryption for data at rest and in transit, and enforce the principle of least privilege and separation of duties to restrict data access strictly to authorized personnel.

* **2. Medium Priority (Short-Term / 30 Days):**
  * **Finding:** There are no disaster recovery plans in place, no routine backups of critical data are being performed, and the network lacks an Intrusion Detection System (IDS).
  * **Action:** Establish an automated backup schedule with off-site storage, draft and test a formal disaster recovery plan for business continuity, and deploy a network IDS to detect malicious network traffic and access.

* **3. Low Priority / Ongoing (Long-Term):**
  * **Finding:** Legacy end-of-life systems lack a regular maintenance schedule with unclear intervention methods, and password policies are nominal without a centralized management system.
  * **Action:** Implement a centralized password manager enforcing modern complexity rules, create a structured maintenance and monitoring calendar for legacy systems, and institute ongoing employee security awareness training, along with adding MFA to increase security posture and help employees access relevant systems and data safely.

---

## 5. CONCLUSION

Botium Toys demonstrates a baseline understanding of operational infrastructure, but requires immediate structural enhancements around data encryption, access control frameworks, and compliance tracking to secure its environment against modern threats.

**Reviewer:** Chris  
**Date:** 26-10-2026
