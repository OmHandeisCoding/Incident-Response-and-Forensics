# Memory Forensics

## Overview

Memory forensics involves analysing the contents of a system's RAM to uncover attacker activity that may not persist to disk — including running malware, encryption keys, network connections, and attacker commands.

---

## When to Use Memory Forensics
- Malware suspected to be **fileless** (living in memory only)
- Ransomware — encryption keys may still be in RAM
- Active attacker — capture their active session artifacts
- Rootkit investigation — processes hidden from OS visible in raw memory
- Credential theft — LSASS memory contains NTLM hashes / cleartext passwords

---

## Step 1: Memory Acquisition

> ⚠️ Must be done on a **live** running system. Memory is volatile — it's lost on reboot.

### Windows Tools
```powershell
# WinPmem (open source)
winpmem.exe memory.raw

# DumpIt (GUI, simple)
DumpIt.exe /output memory.raw

# Belkasoft RAM Capturer
# ProcDump (for a single process)
procdump.exe -ma lsass.exe lsass.dmp
```

### Linux Tools
```bash
# LiME (Linux Memory Extractor) — kernel module
sudo insmod lime.ko "path=/tmp/memory.lime format=lime"

# Using /proc/kcore (limited)
dd if=/proc/kcore of=/tmp/memory.raw
```

**Always verify hash after acquisition:**
```bash
sha256sum memory.raw > memory.raw.sha256
```

---

## Step 2: Analysis with Volatility 3

### Setup
```bash
pip install volatility3
# Get symbol tables: https://github.com/volatilityfoundation/volatility3/releases
```

### Essential Commands

```bash
# Identify OS profile
vol.py -f memory.raw windows.info

# List running processes
vol.py -f memory.raw windows.pslist

# Process tree (parent-child relationships)
vol.py -f memory.raw windows.pstree

# Find hidden/injected processes (compare to pslist)
vol.py -f memory.raw windows.psscan

# Network connections
vol.py -f memory.raw windows.netstat

# Injected code in processes (shellcode, DLL injection)
vol.py -f memory.raw windows.malfind

# DLLs loaded per process
vol.py -f memory.raw windows.dlllist --pid <PID>

# Dump suspicious process executable
vol.py -f memory.raw windows.dumpfiles --pid <PID> -o ./output/

# Command line history
vol.py -f memory.raw windows.cmdline

# Registry hives in memory
vol.py -f memory.raw windows.registry.hivelist

# Extract LSASS credentials (requires Mimikatz or pypykatz post-dump)
vol.py -f memory.raw windows.lsadump
```

---

## Key Memory Indicators of Compromise

| Artifact | What to Look For |
|----------|----------------|
| Process Hollowing | Legitimate process name (e.g., `svchost.exe`) with injected malicious code; parent process mismatch |
| DLL Injection | Unexpected DLLs in trusted processes |
| Malfind Results | RWX memory regions with PE headers — strong indicator of injection |
| Orphaned Processes | Processes with no parent (PPID doesn't exist) |
| Network Connections | Unexpected outbound connections from system processes |
| LSASS Dump | Credential materials in memory (Mimikatz artefacts) |

---

## Tools Reference

| Tool | Purpose | Link |
|------|---------|------|
| Volatility 3 | Memory analysis framework | https://github.com/volatilityfoundation/volatility3 |
| WinPmem | Windows memory acquisition | https://github.com/Velocidex/WinPmem |
| DumpIt | Simple Windows memory capture | Comae Technologies |
| LiME | Linux memory acquisition (kernel module) | https://github.com/504ensicsLabs/LiME |
| pypykatz | Parse LSASS dumps for credentials | https://github.com/skelsec/pypykatz |
| Rekall | Alternative memory analysis framework | https://github.com/google/rekall |
