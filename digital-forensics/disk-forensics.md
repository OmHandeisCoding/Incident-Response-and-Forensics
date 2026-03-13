# Disk Forensics

## Overview

Disk forensics involves acquiring and analysing storage media (HDDs, SSDs, USB) to recover files, establish timelines, identify malicious activity, and support legal proceedings.

---

## Step 1: Evidence Acquisition

### Hardware Write-Blockers
Always use a **write-blocker** to prevent modifying the original evidence drive.
- **Tableau Forensic Bridges** (industry standard)
- **WiebeTech** devices
- Software equivalent: Linux `hdparm -r1 /dev/sdb` (read-only flag)

### Creating a Forensic Image

```bash
# dd (basic)
dd if=/dev/sdb of=/evidence/image.dd bs=4M conv=sync,noerror

# dcfldd (with hashing)
dcfldd if=/dev/sdb of=/evidence/image.dd bs=4M hash=sha256 hashlog=/evidence/image.sha256

# FTK Imager (Windows GUI)
# File → Create Disk Image → Select Source → E01 or RAW format
```

### Supported Image Formats
| Format | Extension | Notes |
|--------|-----------|-------|
| RAW / dd | `.dd`, `.raw`, `.img` | Simple, no compression |
| EnCase | `.E01` | Compressed, metadata, widely accepted in court |
| AFF4 | `.aff4` | Open source, supports encryption |

---

## Step 2: Mounting for Analysis (Read-Only)

```bash
# Linux — mount raw image read-only
sudo mount -o ro,loop,noexec image.dd /mnt/evidence

# Mount E01 with ewfmount
ewfmount image.E01 /mnt/ewf/
mount -o ro,loop /mnt/ewf/ewf1 /mnt/evidence

# Windows — FTK Imager → Image Mounting → Read Only
```

---

## Step 3: File System Analysis

### Autopsy Workflow (GUI)
1. New Case → Add Data Source → Disk Image
2. Run Ingest Modules: Hash Lookup, File Type ID, Keyword Search, EXIF
3. Review: Timeline, Results, Tagged Files

### Sleuth Kit (CLI)
```bash
# Image info
img_stat image.dd

# File system info
fsstat image.dd

# File listing (recursive)
fls -r image.dd

# Metadata for specific inode
istat image.dd <inode>

# File recovery by inode
icat image.dd <inode> > recovered_file
```

---

## Step 4: Recovering Deleted Files

```bash
# Autopsy: Deleted Files section (auto-detected unallocated space)

# PhotoRec (file carving — works on any file type)
photorec image.dd

# Scalpel (signature-based carving)
scalpel -c /etc/scalpel/scalpel.conf image.dd -o /output/carved/

# foremost (file carving)
foremost -t all -i image.dd -o /output/foremost/
```

---

## Step 5: Timeline Analysis

```bash
# Create a bodyfile from the image
fls -r -m / image.dd > bodyfile.txt

# Add MFT data (Windows)
icat image.dd <MFT inode> | analyzeMFT.py -f - -o mft.csv

# Create timeline sorted by timestamp
mactime -b bodyfile.txt -d > timeline.csv

# View in Autopsy → Timeline tab for graphical analysis
```

---

## Step 6: Password & Encryption Analysis

```bash
# Extract Windows hashes with samdump2
samdump2 system sam > hashes.txt

# Or with impacket (from AD dump)
secretsdump.py -ntds ntds.dit -system SYSTEM LOCAL

# Crack with hashcat
hashcat -m 1000 hashes.txt rockyou.txt

# BitLocker — need recovery key or FVEK from memory dump
```

---

## Tools Summary

| Tool | Purpose | Platform |
|------|---------|---------|
| Autopsy | Full GUI forensic suite | Win/Linux |
| Sleuth Kit | CLI filesystem analysis | Linux |
| FTK Imager | Acquisition + preview | Windows (free) |
| Volatility | Memory analysis | Cross-platform |
| PhotoRec / Scalpel | File carving | Cross-platform |
| Plaso / log2timeline | Super timeline creation | Linux |
| RegRipper | Registry analysis | Cross-platform |
| KAPE | Rapid triage artefact collection | Windows |
