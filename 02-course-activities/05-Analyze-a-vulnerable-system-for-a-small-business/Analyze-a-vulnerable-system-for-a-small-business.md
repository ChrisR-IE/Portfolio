> **Note:** For the best visual formatting and layout, you can download the original formal Word document version of this report here: [`Analyze-a-Vulnerable-System-for-a-Small-Business.docx`](./Analyze-a-Vulnerable-System-for-a-Small-Business.docx). This Markdown version is provided for quick, convenient in-browser reading. The markdown was created using AI based off the provided and completed template from the course.

---

# Portfolio Activity: Analyze a Vulnerable System for a Small Business

## Vulnerability Assessment Report
**Date:** 16/11/2026

---

### System Description
The server hardware consists of a powerful CPU processor and 128GB of memory. It runs on the latest version of Linux operating system and hosts a MySQL database management system. It is configured with a stable network connection using IPv4 addresses and interacts with other servers on the network. Security measures include SSL/TLS encrypted connections.

---

### Scope
The scope of this vulnerability assessment relates to the current access controls of the system. The assessment will cover a period of three months, from June 20XX to August 20XX. NIST SP 800-30 Rev. 1 is used to guide the risk analysis of the information system.

---

### Purpose
This vulnerability assessment evaluates the security posture of the company’s remote MySQL database server, which has remained open to the public internet since launching three years ago. The database is a critical business asset because globally distributed remote employees query it daily to identify potential customers and drive e-commerce sales. Securing this server is essential to protect sensitive customer and business records from unauthorized exposure, theft, or manipulation. If the server were compromised or disabled, daily sales operations would halt, resulting in significant financial loss and reputational damage.

---

### Risk Assessment

| Threat source | Threat event | Likelihood | Severity | Risk |
| :--- | :--- | :---: | :---: | :---: |
| Hacker | Obtain sensitive information via exfiltration | 3 | 3 | 9 |
| Competitor | Conduct Denial of Service (DoS) attacks | 2 | 3 | 6 |
| Employee | Alter/Delete critical information | 2 | 3 | 6 |

---

### Approach
A qualitative risk assessment was conducted using NIST SP 800-30 Rev. 1 guidelines to evaluate high-priority threats targeting our publicly exposed database server. External hackers, competitors, and internal employees were selected because open public access makes data exfiltration, Denial of Service attacks, and unauthorized data alteration critical business risks. Likelihood and severity scores were derived using expert judgment to weigh attacker capability against the operational impact of losing customer data or database availability. Because this assessment relies on subjective qualitative estimates rather than live penetration testing, technical vulnerabilities within the underlying Linux OS or MySQL configuration remain outside its scope.

---

### Remediation Strategy
The server currently utilizes SSL/TLS encryption for data in transit, critical access control gaps are needed for immediate remediation using a defense-in-depth approach. First, the database must be removed from public internet exposure by placing it behind a firewall and requiring remote employees to connect via a secure VPN. Second, implementing Multi-Factor Authentication (MFA), the Principle of Least Privilege, and the Authentication, Authorization, and Accounting (AAA) framework will prevent unauthorized access, data exfiltration, and accidental or malicious alterations. Together, these controls will safeguard sensitive customer records and ensure reliable database availability for daily business operations.
