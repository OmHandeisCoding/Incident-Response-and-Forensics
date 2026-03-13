# Incident Classification Matrix

## Severity Tiers

| Priority | Name | Description | Response SLA | Examples |
|----------|------|-------------|-------------|---------|
| **P1** | Critical | Active breach, data exfiltration, ransomware detonation, full system compromise | Immediate — 15 min | Ransomware encryption in progress, confirmed data exfil, APT lateral movement |
| **P2** | High | Significant threat with potential for major impact; attacker may be active | 1 hour | Compromised privileged account, malware on critical server, insider threat activity |
| **P3** | Medium | Threat with limited current impact; no active attacker movement detected | 4 hours | Phishing campaign detected, malware on workstation, unauthorized access attempt |
| **P4** | Low | Minimal risk; likely policy violation or low-confidence alert | 24 hours | Single failed login, suspicious email reported, port scan from external IP |

---

## Escalation Matrix

| Priority | Initial Responder | Escalate To | Notify Management? | Notify Legal/PR? |
|----------|------------------|-------------|-------------------|-----------------|
| P1 | On-call SOC Analyst | IR Lead + CISO | Yes — immediately | Yes (if PII/data involved) |
| P2 | SOC Analyst | IR Lead | Within 1 hour | If data breach suspected |
| P3 | SOC Analyst | Senior Analyst | If unresolved in 4h | No |
| P4 | SOC Analyst | Self-handled | No | No |

---

## Classification Decision Tree

```
Alert Received
    │
    ├─ Is an attacker actively moving through the network?
    │       YES → P1 (Critical)
    │       NO  ↓
    │
    ├─ Is a privileged account or critical asset compromised?
    │       YES → P2 (High)
    │       NO  ↓
    │
    ├─ Is there confirmed malicious activity (malware, phishing, unauthorized access)?
    │       YES → P3 (Medium)
    │       NO  ↓
    │
    └─ Low-confidence alert / policy violation
            → P4 (Low)
```

---

## Incident Categories

| Category | Examples |
|----------|---------|
| Malware | Ransomware, spyware, trojans, rootkits |
| Phishing | Business Email Compromise (BEC), credential harvest, spear phishing |
| Unauthorized Access | Brute force, credential stuffing, privilege escalation |
| Insider Threat | Data theft, sabotage, policy violation |
| Denial of Service | DDoS, application-level DoS |
| Data Breach | Confirmed exfiltration of PII, PHI, PCI, IP |
| Supply Chain | Compromised vendor software or hardware |

---

## Evidence to Collect at Classification

- Alert details (source, time, rule triggered)
- Affected hosts / user accounts
- Network connections (source/destination IPs, ports)
- Process trees (parent/child processes)
- Initial IOCs (file hashes, domains, IPs)
