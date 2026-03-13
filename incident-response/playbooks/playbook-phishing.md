# Playbook: Phishing Incident Response

**Playbook ID:** PB-001  
**Playbook Type:** Phishing / Business Email Compromise (BEC)  
**Severity:** P3 (Medium) → escalate to P2 if credentials confirmed compromised  
**Owner:** SOC Analyst / IR Team

---

## 1. Detection Triggers

- User reports suspicious email
- Email gateway alert (malicious link, attachment, spoofed sender)
- SIEM alert: login from unfamiliar IP after clicking email link
- Threat intel IOC match on email sender/domain/URL

---

## 2. Initial Triage (Target: <30 minutes)

- [ ] Obtain the original email (header + body) from user or email gateway
- [ ] Extract: sender address, reply-to, originating IP, links, attachments
- [ ] Check sender domain — real or lookalike? (e.g., `microsoft-support.net`)
- [ ] Analyse links: run through VirusTotal, URLscan.io, Cisco Talos
- [ ] Analyse attachments: submit hashes/files to VirusTotal / sandbox
- [ ] Determine: Did any user click the link? Submit credentials?
- [ ] Check email gateway logs for delivery scope — how many users received it?
- [ ] Classify severity and declare incident if confirmed malicious

---

## 3. Containment

### Immediate (within 1 hour of confirmation)
- [ ] Quarantine / recall the phishing email from all mailboxes (via email admin tools)
- [ ] Block the sender domain and originating IP at the email gateway
- [ ] Block malicious URLs at web proxy/DNS filter
- [ ] Notify all recipients: "Do not click links in email from [sender] — security team investigating"

### If Credentials Were Entered
- [ ] Immediately reset password for affected user(s)
- [ ] Revoke active sessions / OAuth tokens
- [ ] Enforce MFA re-enrolment if MFA was not in place
- [ ] Check for email forwarding rules created by attacker (common BEC tactic)
- [ ] Review email sent-items and mailbox rules for attacker persistence

---

## 4. Investigation & Analysis

- [ ] Pull sign-in logs for affected user(s) — focus on impossible travel, new device, unfamiliar IP
- [ ] Check for lateral movement: were credentials reused on other systems?
- [ ] Review Azure AD / Okta sign-in events for the affected account
- [ ] Check if attacker accessed SharePoint, OneDrive, email for data exfiltration
- [ ] Identify full list of affected users (anyone who clicked or submitted credentials)
- [ ] Document all IOCs (sender, IPs, URLs, hashes)

---

## 5. Eradication

- [ ] Ensure email is fully purged from all mailboxes
- [ ] Remove any attacker-created inbox rules or forwarding
- [ ] Remove any OAuth app consents granted during the attack
- [ ] Revoke any compromised tokens or app passwords
- [ ] Patch or update email gateway rules to block similar future campaigns

---

## 6. Recovery

- [ ] Confirm clean access for affected users
- [ ] Monitor affected accounts for 30 days for re-compromise signs
- [ ] Brief affected users on what happened and phishing awareness tips
- [ ] Update threat intel feeds with IOCs from this incident

---

## 7. Post-Incident Steps

- [ ] Complete IR Report (use ir-report-template.md)
- [ ] Conduct lessons learned session with SOC team
- [ ] Update email filtering rules
- [ ] Consider phishing simulation campaign to test org readiness

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Initial Access | Phishing | T1566 |
| Credential Access | Steal Web Session Cookie | T1539 |
| Persistence | Email Forwarding Rule | T1114.003 |
| Collection | Email Collection | T1114 |
| Exfiltration | Exfiltration Over Web Service | T1567 |
