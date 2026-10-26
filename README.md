# Automated Triage & Forensic Analysis of Phishing Threats

## 🎯 Project Objective
This project demonstrates the end-to-end incident response lifecycle for triaging, investigating, and containing phishing threats. Utilizing a realistic Security Operations Center (SOC) scenario, this repository showcases the technical methodology required to analyze malicious email headers, evaluate infrastructure reputation, decode threat actor pretexts, and implement robust perimeter defenses.

---

## 🛠️ Defensive Tools & Skills Matrix

| Category | Infrastructure & Applications Analyzed |
| :--- | :--- |
| **Forensic Analysis** | Email Header Parsing, Reverse DNS (rDNS) Verification |
| **Threat Intelligence** | VirusTotal, Domain Reputation Triage |
| **Dynamic Sandboxing** | URL2PNG Visual Intent Inspection |

---

## 🧭 Core Incident Response Process
The investigation strictly follows a six-phase operational playbook to ensure systematic containment:
1. **Retrieve** an original copy of the suspicious email safely.
2. **Gather** artifacts (headers, routing metadata, and web targets).
3. **Inform** targeted recipients to prevent accidental exposure.
4. **Investigate** malicious artifacts using threat intelligence and sandboxes to extract Indicators of Compromise (IoCs).
5. **Take** defensive measures (DNS sinkholing, proxy blocks).
6. **Complete** a comprehensive technical incident analysis report.

---

## 📊 Investigation Summary & Impact
*   **Threat Type:** Malicious Phishing & Credential Harvesting Campaign
*   **Pretext:** Masquerading as an urgent Microsoft Outlook Security Notice
*   **Target Scope:** Identified 5 distinct corporate mailboxes targeted simultaneously.
*   **Final Verdict:** **MALICIOUS**

---

## 📂 Repository Directory Structure
*   **`Analysis_Report.md`**: The formal technical report covering email descriptions, deep artifact analysis, and tactical remediation steps.
*   **`Phishing_Playbook.md`**: The step-by-step procedural workflow utilized during the triage process.
*   **`ioc_list.txt`**: A centralized, completely defanged manifest of extracted threat data (IPs, domains, URLs) formatted for security gateway ingestion.
*   **`evidence/`**: Supporting forensic proofs, including parsed header metrics and sandbox interface captures.

---
*Disclaimer: All analysis notes and artifacts have been sanitized, generalized, and defanged to maintain structural integrity and comply with security training non-disclosure agreements (NDAs).*
