# Incident Response Report Template

**Classification:** CONFIDENTIAL  
**Report Version:** 1.0  
**Date:** [DATE]  
**Prepared By:** [ANALYST NAME]  
**Incident ID:** IR-[YEAR]-[NUMBER]

---

## 1. Executive Summary

| Field | Details |
|-------|---------|
| Incident Title | [Brief descriptive title] |
| Severity | P1 / P2 / P3 / P4 |
| Date Detected | [DATE TIME TZ] |
| Date Contained | [DATE TIME TZ] |
| Date Resolved | [DATE TIME TZ] |
| MTTD | [X hours/minutes] |
| MTTR | [X hours/minutes] |
| Systems Affected | [List of hostnames/IPs] |
| Users Affected | [Number and roles] |
| Data Compromised | Yes / No / Under Investigation |

**Summary (2–3 sentences):**  
[Describe what happened, how it was detected, and the outcome at a level suitable for non-technical leadership.]

---

## 2. Incident Timeline

| Date/Time (UTC) | Event | Actor | Evidence Source |
|-----------------|-------|-------|----------------|
| [YYYY-MM-DD HH:MM] | Initial compromise vector | Attacker | Firewall log, EDR |
| [YYYY-MM-DD HH:MM] | Lateral movement detected | Attacker | SIEM alert |
| [YYYY-MM-DD HH:MM] | Alert triggered in SIEM | SOC Analyst | SIEM |
| [YYYY-MM-DD HH:MM] | Incident declared | IR Lead | IR ticket |
| [YYYY-MM-DD HH:MM] | Containment actions taken | IR Team | Change log |
| [YYYY-MM-DD HH:MM] | Systems restored | IT/IR | Change log |
| [YYYY-MM-DD HH:MM] | Incident closed | IR Lead | IR ticket |

---

## 3. Technical Details

### 3.1 Attack Vector
[Describe how the attacker gained initial access — phishing, exposed RDP, supply chain, etc.]

### 3.2 Attack Narrative
[Step-by-step technical account of what occurred, including TTPs observed and mapped to MITRE ATT&CK where applicable.]

**MITRE ATT&CK Mapping:**
| Tactic | Technique | Technique ID |
|--------|-----------|-------------|
| Initial Access | Phishing | T1566 |
| Persistence | [Technique] | [ID] |
| Lateral Movement | [Technique] | [ID] |

### 3.3 Systems Affected
| Hostname | IP Address | Role | Impact |
|----------|-----------|------|--------|
| [host1] | [IP] | [e.g., File Server] | [Compromised / Accessed / Encrypted] |

---

## 4. Indicators of Compromise (IOCs)

| Type | Value | Confidence | Notes |
|------|-------|-----------|-------|
| IP Address | [x.x.x.x] | High | C2 server |
| Domain | [malicious.domain.com] | High | Phishing sender |
| File Hash (SHA256) | [hash] | High | Malware sample |
| Registry Key | [key path] | Medium | Persistence mechanism |

---

## 5. Containment & Eradication Actions

| Action | Owner | Date/Time | Status |
|--------|-------|-----------|--------|
| Isolated [host1] from network | [Analyst] | [DATE] | ✅ Done |
| Disabled compromised user account | [Admin] | [DATE] | ✅ Done |
| Blocked IOC IPs at firewall | [NetEng] | [DATE] | ✅ Done |
| Removed malware from [host1] | [IR Team] | [DATE] | ✅ Done |
| Reset credentials for affected accounts | [Admin] | [DATE] | ✅ Done |
| Patched exploited vulnerability | [IT] | [DATE] | ✅ Done |

---

## 6. Root Cause Analysis

**Root Cause:**  
[Single sentence identifying the fundamental cause — e.g., "An unpatched RDP vulnerability (CVE-XXXX-XXXX) on a public-facing server allowed initial access."]

**Contributing Factors:**
- [Factor 1 — e.g., Lack of MFA on remote access]
- [Factor 2 — e.g., Delayed patch deployment process]
- [Factor 3 — e.g., No network segmentation between DMZ and internal]

---

## 7. Business Impact Assessment

| Impact Area | Assessment |
|-------------|-----------|
| Data Loss | [None / [X] records of [type]] |
| Regulatory Exposure | [None / GDPR / HIPAA / PCI DSS] |
| Financial Impact | [Estimated $X in downtime / remediation] |
| Reputational Impact | [Low / Medium / High] |
| Operational Downtime | [X hours of [system/service]] |

---

## 8. Lessons Learned & Recommendations

| # | Finding | Recommendation | Owner | Target Date |
|---|---------|---------------|-------|------------|
| 1 | [e.g., No MFA on VPN] | Enforce MFA for all remote access | IT Security | [DATE] |
| 2 | [e.g., Delayed detection] | Tune SIEM rules for [technique] | SOC Lead | [DATE] |
| 3 | [e.g., Poor segmentation] | Implement VLANs between DMZ and internal | Network | [DATE] |

---

## 9. Appendices

- **Appendix A:** Raw log evidence
- **Appendix B:** Forensic artifacts collected
- **Appendix C:** Communication records
- **Appendix D:** Chain of custody forms

---

*This report is classified CONFIDENTIAL and should be distributed only on a need-to-know basis.*
