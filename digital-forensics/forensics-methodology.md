# Digital Forensics Methodology

## Core Principles — ACPO Guidelines

The **ACPO (Association of Chief Police Officers) Principles** govern forensic evidence handling:

| Principle | Description |
|-----------|-------------|
| **1** | No action should change data on original media that may later be relied upon in court |
| **2** | In exceptional circumstances where access to original data is necessary, that person must be competent and able to explain their actions |
| **3** | An audit trail must be created and preserved; a third party should be able to examine and achieve the same results |
| **4** | The officer in charge has overall responsibility for ensuring these principles are followed |

---

## Chain of Custody

Every piece of evidence must be documented from collection to courtroom.

### Chain of Custody Form — Required Fields

| Field | Description |
|-------|-------------|
| Item Number | Unique evidence ID (e.g., EV-2024-001) |
| Description | Device type, make, model, serial number |
| Date/Time Collected | UTC timestamp |
| Collected By | Name + role |
| Collection Method | Logical acquisition / Physical imaging |
| Hash Value (MD5/SHA256) | Taken immediately after collection |
| Storage Location | Evidence locker ID / Secure storage path |
| Transfer Log | Each person who received/transferred the evidence, with date/time |

### Hash Verification
```
# On collection
sha256sum /dev/sda > original_drive.sha256

# Before and after every analysis session
sha256sum evidence.dd | diff - original_drive.sha256
```

---

## Evidence Integrity

- **Always** work on a **forensic copy**, never the original
- Use **write-blockers** when imaging physical media
- Verify hash of image matches hash of original before analysis
- Store originals in anti-static bags in secure evidence locker
- Log every access to evidence

---

## Forensic Acquisition Methods

| Method | Description | Use When |
|--------|-------------|----------|
| Physical (bitwise) | Sector-by-sector copy of entire disk | Criminal investigation, maximum evidence |
| Logical | File system-level copy | Civil matters, quick triage |
| Live Acquisition | Capturing volatile data from running system | Active IR, memory forensics |
| Network Forensics | Capturing traffic / log analysis | Exfiltration investigation |

### Tools for Acquisition
| Tool | Type | Notes |
|------|------|-------|
| FTK Imager | Disk imaging | Free, widely used, GUI |
| dd / dcfldd | Disk imaging | Linux CLI, scriptable |
| Guymager | Disk imaging | Linux GUI, open source |
| WinPmem / DumpIt | Memory acquisition | Windows RAM dump |
| Velociraptor | Remote live acquisition | Enterprise DFIR agent |

---

## Forensic Analysis Workflow

```
1. Receive/document evidence → 2. Verify integrity (hash) → 3. Create forensic copy
       ↓
4. Mount copy (read-only) → 5. Timeline analysis → 6. Identify artifacts
       ↓
7. Recover deleted files → 8. Correlate findings → 9. Document & report
```

---

## References
- ACPO Good Practice Guide for Digital Evidence
- NIST SP 800-86: Guide to Integrating Forensic Techniques into IR
- RFC 3227: Guidelines for Evidence Collection and Archiving
