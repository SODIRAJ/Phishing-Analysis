## Section 1: Email Description and Artifacts Collected

### Email Description
The suspicious message is themed as a critical security alert masquerading as an official notification from Outlook. The email body claims that the target mailbox is at risk, utilizing a high-urgency psychological pretext ("Act NOW!") to induce panic and force immediate user compliance. The underlying objective of this campaign is malicious credential harvesting via a fake Outlook Web Access (OWA) login page. 

---

### Artifacts Collected

#### 1. Email-based Artifacts
*   **Sender Display Name:** *Not explicitly stated in raw metadata, but sender claims to be Outlook Security Alerts.*
*   **Sending Address:** `emailsecalert1@gmail.com`
*   **Reply-To Address:** `emailsecalert1@gmail.com`
*   **Subject Line:** `Your Email Will be Locked! Act NOW!`
*   **Sending Server IP:** `209.85.222.173`
*   **Reverse DNS (rDNS):** `://google.com` (Verified originating infrastructure: Gmail)
*   **Date and Time received:** 3:21 PM Monday, 1st June 2020
*   **Targeted Recipients:**
    *   `john.smith@dicksonunited.co.uk`
    *   `alice.cooper@dicksonunited.co.uk`
    *   `jacon.long@dicksonunited.co.uk`
    *   `fred.johnson@dicksonunited.co.uk`
    *   `pickle.rick@dicksonunited.co.uk`

#### 2. Web-based Artifacts
*   **Full Malicious URL (Defanged):** `hxxps://outlook-security.emailsecalerts[.]net/index/2020/OWA.php?`
*   **Root Domain (Defanged):** `emailsecalerts[.]net`

#### 3. File-based Artifacts
*   **Attachment Status:** None. This campaign relies purely on web-based credential harvesting hyperlinks.

---

## Section 2: Artifact Analysis

### Actions Taken & Investigation Tools Used
1.  **Email Origin Verification (Reverse DNS):**
    *   **Action:** Performed a Reverse DNS (rDNS) lookup on the sending server IP address (`209.85.222.173`).
    *   **Tool / Finding:** Confirmed the pointer record points to `://google.com`. This verifies the email originated from legitimate Google (Gmail) infrastructure rather than official Microsoft exchange environments, exposing explicit external masquerading.
2.  **Visual and Intent Inspection (URL2PNG):**
    *   **Action:** Submitted the embedded link to URL2PNG to safely inspect the web layout without risking direct browser exposure or session tracking.
    *   **Tool / Finding:** The sandbox capture revealed a spoofed interface mimicking an **Outlook Web Access (OWA)** portal, confirming active credential harvesting intent.
3.  **Reputation and Threat Intelligence Triage (VirusTotal):**
    *   **Action:** Checked the reputation of the malicious root domain (`emailsecalerts[.]net`) against threat intelligence aggregators.
    *   **Tool / Finding:** VirusTotal flagged the domain for malicious and phishing behavior, validating that the infrastructure is known threat actor infrastructure.
4.  **Internal Impact Assessment (SIEM and EDR Log Auditing):**
    *   **Action:** Queried centralized SIEM and Endpoint Detection & Response (EDR) logs using the malicious URL/domain strings, and reviewed email gateway records for inbound/outbound responses.
    *   **Tool / Finding:** Cross-referenced traffic and mailbox data to identify potential exposure or compromise within the network.

---

### Analysis Results and Findings
*   **Infrastructure Verdict:** The root domain `emailsecalerts[.]net` is classified as **Malicious**. Threat intelligence history and sandbox analysis prove this domain exists purely to host deceptive landing pages designed for credential harvesting.
*   **Scope of Impact:** Internal security logging confirmed that while the phishing campaign targeted 5 distinct corporate mailboxes (`john.smith`, `alice.cooper`, `jacon.long`, `fred.johnson`, and `pickle.rick`), **no users clicked on the link or created a network session with the malicious domain**.
*   **Communication Audit:** Email gateway logs indicate that zero users have replied to the malicious external address (`emailsecalert1@gmail.com`). 
*   **Final Assessment:** The attack failed to achieve execution. There is zero evidence of network infiltration, user data exposure, or active host compromise.

---

## Section 3: Suggested Defensive Measures

### Remediation and Containment Actions

1.  **Network-Layer Defenses (DNS & Web Proxy Blocklisting):**
    *   Implement an immediate block on the root domain `emailsecalerts[.]net` and all associated subdomains across the corporate web proxy and secure DNS gateway (e.g., Cisco Umbrella, Zscaler). This ensures that even if a user tries to click the link later, the connection will be dropped safely at the perimeter.
2.  **Email Gateway Restrictions (Inbound Mail Controls):**
    *   Add the specific malicious URL string (`hxxps://outlook-security.emailsecalerts[.]net/index/2020/OWA.php?`) to the email gateway filter rule to block future delivery attempts.
    *   Add the specific sender address `emailsecalert1@gmail.com` to the tenant blocklist.
3.  **Active Mailbox Quarantine:**
    *   Execute an administrative purge across the email environment to permanently remove the phishing message from the 5 targeted user inboxes (`john.smith`, `alice.cooper`, `jacon.long`, `fred.johnson`, and `pickle.rick`) to eliminate the risk of delayed interactions.

---

### Defensive Operational Impact Considerations

When implementing these security controls, the following operational trade-offs were evaluated to prevent business disruption:

*   **Blocking the Threat Infrastructure (`emailsecalerts[.]net`):** 
    *   *Impact Assessment:* **Zero Risk.** Threat intelligence and sandbox analysis prove this domain is entirely malicious and holds no legitimate business utility. Blocking it will cause no false positives.
*   **Handling the Sending Domain (`gmail.com`):**
    *   *Impact Assessment:* **High Risk.** While the attacker used a Gmail address (`emailsecalert1@gmail.com`), the sending server IP (`209.85.222.173`) and root domain belong to legitimate Google infrastructure. 
    *   *Mitigation:* **Do not block the sending IP or the root domain `gmail.com`**, as doing so would block millions of legitimate external emails from clients, vendors, and partners. Perimeter blocks must remain strictly limited to the specific threat actor email alias and malicious target URLs.