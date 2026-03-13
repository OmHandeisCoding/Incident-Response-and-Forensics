# Incident Response Lifecycle — NIST SP 800-61r2

## Overview

NIST SP 800-61r2 defines four core phases of the Incident Response lifecycle:

```
Preparation → Detection & Analysis → Containment, Eradication & Recovery → Post-Incident Activity
```

---

## Phase 1: Preparation

**Goal:** Establish the capability to respond effectively before an incident occurs.

### Key Activities
- Establish and train an Incident Response Team (IRT)
- Develop and maintain IR policies, procedures, and playbooks
- Deploy detection tools: SIEM, EDR, IDS/IPS, log aggregation
- Conduct tabletop exercises and simulations
- Maintain an up-to-date asset inventory and network diagrams
- Establish communication channels and escalation contacts
- Configure jump bags / forensic toolkits

### Deliverables
- IR Policy Document
- Contact List (internal + external: legal, PR, law enforcement)
- Playbooks for known threat types
- SIEM/EDR baseline rules

---

## Phase 2: Detection & Analysis

**Goal:** Identify and confirm that an incident has occurred, and determine its scope.

### Detection Sources
| Source | Examples |
|--------|---------|
| SIEM Alerts | Failed logins, lateral movement, data exfil patterns |
| EDR/AV | Malware detection, process injection, persistence mechanisms |
| User Reports | Phishing emails, unusual behavior |
| Threat Intel | IOC matches from threat feeds |
| Network Monitoring | Unusual outbound traffic, beaconing |

### Analysis Steps
1. **Triage** — Determine if alert is true positive or false positive
2. **Scope** — Identify affected systems, users, and data
3. **Classify** — Assign severity using the classification matrix (P1–P4)
4. **Document** — Start the IR timeline and evidence log
5. **Notify** — Alert relevant stakeholders per escalation path

### Key Questions to Answer
- What happened? When?
- Which systems/users are affected?
- Is the attacker still active?
- What data may have been accessed or exfiltrated?

---

## Phase 3: Containment, Eradication & Recovery

### 3a. Containment
**Short-term containment** (immediate stop-the-bleeding):
- Isolate affected systems from the network
- Disable compromised accounts
- Block malicious IPs/domains at firewall/proxy

**Long-term containment** (maintain business while cleaning up):
- Implement enhanced monitoring on adjacent systems
- Apply emergency patches
- Deploy honeypots if attacker is still active

### 3b. Eradication
- Remove malware, backdoors, and unauthorized accounts
- Patch exploited vulnerabilities
- Validate clean state of affected systems
- Reset credentials for all compromised accounts

### 3c. Recovery
- Restore systems from clean backups (verify integrity first)
- Monitor restored systems closely for 30–90 days
- Gradually return to normal operations
- Confirm no indicators of compromise remain

---

## Phase 4: Post-Incident Activity

**Goal:** Learn from the incident to improve future response.

### Lessons Learned Meeting
- Conduct within 2 weeks of incident closure
- Attendees: IR team, IT, management, affected business units
- Review: What happened, what worked, what didn't, what to improve

### Post-Incident Report
- Executive summary
- Full timeline of events
- Root cause analysis
- Indicators of Compromise (IOCs)
- Remediation actions taken
- Recommendations

### Metrics to Track
| Metric | Description |
|--------|-------------|
| MTTD | Mean Time to Detect |
| MTTR | Mean Time to Respond/Recover |
| Incident Volume | Incidents per category per month |
| False Positive Rate | % of alerts that are false positives |

---

## References
- NIST SP 800-61r2: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf
- SANS Incident Handler's Handbook
- CISA Incident Response Playbooks
