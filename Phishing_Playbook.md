# Phishing Analysis Immediate Response Playbook

This playbook outlines the standard operational procedure I used for triaging, investigating, and responding to this suspected phishing emails.

---

## 🧭 Core Incident Response Process

Follow these six mandatory sequential steps once a suspicious email alert is generated:

1. **Retrieve an original copy of the phishing email.**
   * Secure the raw `.eml` or `.msg` file safely without opening it directly in a live mail client.

2. **Gather artifacts from the phishing email.**
   * Extract header metadata, delivery routes, sender addresses, visible links, and attachment hashes.

3. **Inform the recipients that received the email.**
   * Notify affected users to prevent accidental interaction and instruct them to isolate the message.

4. **Investigate malicious artifacts to collect Indicators of Compromise (IoCs).**
   * Analyze URLs and attachments using sandboxes and reputation tools to gather data that can be blocked to protect the organization.

5. **Take defensive measures.**
   * Implement containment actions such as blocklisting malicious domains.

6. **Complete the investigation report.**
   * Document all of the above findings and actions taken in the final incident analysis report.
