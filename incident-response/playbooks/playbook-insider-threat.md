# Playbook: Insider Threat Incident Response

**Playbook ID:** PB-003  
**Severity:** P2 (High) — escalate to P1 if active data exfiltration confirmed  
**Owner:** IR Lead + HR + Legal

---

## 1. Detection Triggers
- DLP alert: large data uploads to personal cloud (Google Drive, Dropbox, USB)
- UEBA alert: anomalous access to systems outside user's normal scope
- HR escalation: employee gave notice / was terminated / under disciplinary action
- Peer report: colleague observed data theft or unusual behaviour
- SIEM: bulk download of files, after-hours access, mass printing

---

## 2. Key Considerations

> ⚠️ Insider threat investigations are **sensitive and legally complex**.
> - **Do NOT confront the employee** without HR and Legal approval
> - **Preserve** evidence rather than immediately blocking access (to avoid tipping off the subject)
> - Engage HR and Legal **before** any containment actions that affect the employee

---

## 3. Initial Assessment (Covert Phase)

- [ ] Brief: IR Lead, HR, Legal, CISO — establish investigation team
- [ ] Define: is this **malicious** (intentional theft) or **negligent** (accidental policy violation)?
- [ ] Pull and preserve logs without alerting the subject:
  - DLP logs
  - Email access logs (sent items, forwarding rules)
  - Cloud storage sync logs (OneDrive, SharePoint)
  - Badge access records
  - VPN / endpoint activity logs
- [ ] Identify: what data was accessed or taken? Classification of that data?
- [ ] Establish a monitoring window — observe behaviour before taking action

---

## 4. Containment (With HR/Legal Approval)

**If employee is still active:**
- [ ] Coordinate with HR for employment action (suspension, termination)
- [ ] Upon HR signal: revoke all access simultaneously (AD, email, VPN, cloud apps)
- [ ] Physically escort employee from premises (coordinate with physical security)
- [ ] Preserve the employee's workstation as forensic evidence — do not wipe

**Data Recovery:**
- [ ] Identify destination of exfiltrated data (USB, personal email, cloud)
- [ ] Legal may issue a litigation hold or preservation order
- [ ] Work with legal to pursue recovery of company data

---

## 5. Forensic Evidence Collection

- [ ] Create forensic image of employee's workstation (bitwise copy)
- [ ] Preserve all relevant logs with hash verification
- [ ] Document chain of custody for all evidence
- [ ] Collect: email archive, browser history, USB device history, print logs
- [ ] Engage external forensic firm if criminal prosecution is likely

---

## 6. Post-Incident Review

- [ ] Brief leadership on findings (keep details confidential / need-to-know)
- [ ] Assess: what data was actually exfiltrated? Notify regulators if required
- [ ] Review: did pre-existing controls fail? (DLP rules, access reviews, offboarding process)
- [ ] Update offboarding checklist (immediate revocation, equipment retrieval)
- [ ] Conduct access review — are other employees with similar access posing risk?

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Collection | Data from Local System | T1005 |
| Exfiltration | Exfiltration Over Web Service | T1567 |
| Exfiltration | Exfiltration to Removable Media | T1052 |
| Defense Evasion | Use Alternate Authentication Material | T1550 |
