# Playbook: Ransomware Incident Response

**Playbook ID:** PB-002  
**Severity:** P1 — Critical  
**Owner:** IR Lead + SOC Team

---

## 1. Detection Triggers
- EDR alert: mass file encryption, shadow copy deletion, ransom note dropped
- User reports files renamed with unknown extension, ransom note on desktop
- SIEM alert: `vssadmin delete shadows`, `wbadmin delete catalog`, PowerShell encoded commands
- Network monitoring: unusual SMB traffic, lateral movement across file shares

---

## 2. Immediate Actions (First 15 Minutes)

> ⚠️ Do NOT reboot systems — volatile memory may contain encryption keys or attacker artifacts.

- [ ] **ISOLATE** affected systems from the network immediately (pull cable / disable NIC)
- [ ] Alert IR Lead and escalate to P1
- [ ] Notify CISO and relevant management
- [ ] Preserve volatile memory if forensically feasible (RAM dump)
- [ ] Identify: Is encryption still in progress? On how many systems?
- [ ] Check backup systems — are they **isolated** and **unaffected**?

---

## 3. Containment

- [ ] Identify patient zero (first infected system) via EDR telemetry
- [ ] Map lateral movement: which systems did the infection spread to?
- [ ] Isolate all affected systems — segment entire subnet if needed
- [ ] Disable compromised or suspect accounts
- [ ] Block ransomware C2 IPs/domains at firewall and DNS
- [ ] Suspend automated backup jobs if ransomware may spread to backup targets
- [ ] Engage legal counsel if data exfiltration likely (double-extortion scenario)

---

## 4. Investigation

- [ ] Identify ransomware variant (check ransom note, extension, ID Ransomware: https://id-ransomware.malwarehunterteam.com/)
- [ ] Determine attack vector: phishing, exposed RDP, vulnerable VPN, compromised credentials
- [ ] Review EDR telemetry for:
  - Initial process tree (how did the payload execute?)
  - Persistence mechanisms (scheduled tasks, registry keys, services)
  - Lateral movement (PsExec, WMI, SMB)
  - Data exfiltration before encryption (double-extortion check)
- [ ] Collect and preserve IOCs: hashes, C2 IPs, domains, attacker tools

---

## 5. Eradication & Recovery

- [ ] Confirm backups are clean and from a pre-infection point-in-time
- [ ] Rebuild affected systems from clean images (do not restore in-place)
- [ ] Restore data from verified clean backups
- [ ] Patch the initial attack vector before reconnecting systems
- [ ] Reset ALL credentials (assume all passwords compromised)
- [ ] Implement / enforce MFA on all remote access
- [ ] Scan remaining systems for persistence before rejoining network

---

## 6. Communication

| Stakeholder | When | Channel |
|-------------|------|---------|
| CISO | Immediately | Phone |
| Legal | Within 1 hour | Phone |
| Exec Leadership | Within 2 hours | Executive briefing |
| Regulatory Bodies | Per breach notification SLA | Written |
| Cyber Insurance | Within 24 hours | Written |
| Law Enforcement | If data exfil confirmed | FBI IC3 / CISA |

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Initial Access | Phishing / Exploit Public-Facing Application | T1566 / T1190 |
| Execution | PowerShell / WMI | T1059.001 / T1047 |
| Persistence | Scheduled Task / Registry Run Key | T1053 / T1547 |
| Defense Evasion | Delete Shadow Copies | T1490 |
| Lateral Movement | SMB/Windows Admin Shares | T1021.002 |
| Exfiltration | Exfiltration to Cloud Storage | T1567 |
| Impact | Data Encrypted for Impact | T1486 |
