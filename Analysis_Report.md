## Section 1: Email Description and Artifacts Collected

### Email Description
The suspicious message is themed as a critical security alert masquerading as an official notification from Outlook. The email body claims that the target mailbox is at risk, utilizing a high-urgency psychological pretext ("Act NOW!") to induce panic and force immediate user compliance.
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

1. **Email Origin Verification (Reverse DNS):**
   * **Action:** Performed a Reverse DNS (rDNS) lookup on the sending server IP address (`209.85.222.173`).
   * **Tool / Finding:** Confirmed the pointer record points to `://google.com`. This verifies the email originated from legitimate Google (Gmail) infrastructure rather than official Microsoft Exchange environments, exposing explicit external masquerading. *(Visual Evidence saved in `evidence/headers/rdns_lookup.png`)*

2. **Reputation and Threat Intelligence Triage (VirusTotal):**
   * **Action:** Checked the historical and live reputation of the malicious root domain (`emailsecalerts[.]net`) against threat intelligence aggregators.
   * **Tool / Finding:** VirusTotal flagged the domain for active malicious and phishing behavior, validating that the underlying infrastructure is known threat actor infrastructure. *(Visual Evidence saved in `evidence/sandbox_results/virustotal_reputation.png`, `evidence/sandbox_results/virustotal_reputation2.png`)*

3. **Active Visual & Intent Inspection (Perimeter URL Check):**
   * **Action:** Attempted to pull an isolated web screenshot of the landing page endpoint using web triage utilities to confirm layout configuration.
   * **Tool / Finding:** The target site failed to resolve, experiencing a persistent network timeout. This indicates the malicious hosting setup or the explicit URL path was actively taken down or dismantled by upstream network providers prior to final incident closure. 

---

### Analysis Results

* **Infrastructure Verdict:** The root domain `emailsecalerts[.]net` is classified as **Malicious**. Even though the live URL endpoint is down due to a successful infrastructure takedown, threat intelligence history proves this domain was registered and deployed purely to host deceptive web applications designed for credential harvesting.
*
  

---

## Section 3: Suggested Defensive Measures

### Remediation and Containment Actions

1.  **Network-Layer Defenses (DNS & Web Proxy Blocklisting):**
    *   Implement an immediate block on the root domain `emailsecalerts[.]net` and all associated subdomains across the corporate web proxy and secure DNS gateway. This ensures that even if a user tries to click the link later, the connection will be dropped safely at the perimeter.
2.  **Email Gateway Restrictions (Inbound Mail Controls):**
    *   Add the specific malicious URL string (`hxxps://outlook-security.emailsecalerts[.]net/index/2020/OWA.php?`) to the email gateway filter rule to block future delivery attempts.
    *   Add the specific sender address `emailsecalert1@gmail.com` to the tenant blocklist.

---

### Defensive Operational Impact Considerations

When implementing these security controls, the following operational trade-offs were evaluated to prevent business disruption:

*   **Blocking the Threat Infrastructure (`emailsecalerts[.]net`):** 
    *   *Impact Assessment:* **Zero Risk.** Threat intelligence and sandbox analysis prove this domain is entirely malicious and holds no legitimate business utility. Blocking it will cause no false positives.
*   **Handling the Sending Domain (`gmail.com`):**
    *   *Impact Assessment:* **High Risk.** While the attacker used a Gmail address (`emailsecalert1@gmail.com`), the sending server IP (`209.85.222.173`) and root domain belong to legitimate Google infrastructure. 
    *   *Mitigation:* **Do not block the sending IP or the root domain `gmail.com`**, as doing so would block millions of legitimate external emails from clients, vendors, and partners. Perimeter blocks must remain strictly limited to the specific threat actor email alias and malicious target URLs.
