# Windows Forensics Checklist

## Key Artifacts to Collect

### 1. Event Logs
| Log | Path | Key Events |
|-----|------|-----------|
| Security | `%SystemRoot%\System32\winevt\Logs\Security.evtx` | 4624 (Logon), 4625 (Failed), 4648, 4672, 4688 (Process) |
| System | `%SystemRoot%\System32\winevt\Logs\System.evtx` | Service installs, crashes, driver loads |
| Application | `%SystemRoot%\System32\winevt\Logs\Application.evtx` | App crashes, AV events |
| PowerShell | `Microsoft-Windows-PowerShell/Operational.evtx` | 4103, 4104 (Script block logging) |
| Sysmon | `Microsoft-Windows-Sysmon/Operational.evtx` | Process creation, network, registry |

### 2. Registry Hives
| Hive | Path | Forensic Value |
|------|------|---------------|
| SYSTEM | `%SystemRoot%\System32\config\SYSTEM` | USB history, network config, services |
| SOFTWARE | `%SystemRoot%\System32\config\SOFTWARE` | Installed software, run keys |
| SAM | `%SystemRoot%\System32\config\SAM` | Local user accounts (needs NTLM cracking) |
| NTUSER.DAT | `%UserProfile%\NTUSER.DAT` | User activity, recent docs, typed URLs |
| USRCLASS.DAT | `%UserProfile%\AppData\Local\Microsoft\Windows\UsrClass.dat` | ShellBags, folder access |

**Key Registry Keys for Persistence:**
```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKLM\SYSTEM\CurrentControlSet\Services   # Malicious services
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon  # Userinit, Shell manipulation
```

### 3. Prefetch Files
- **Location:** `%SystemRoot%\Prefetch\`
- **Format:** `APPLICATIONNAME-HASH.pf`
- **Value:** Proves a program was executed, includes last 8 run times and referenced files
- **Tool:** PECmd by Eric Zimmermann (https://github.com/EricZimmermann/PECmd)

### 4. Shellbags
- **Registry:** `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`
- **Value:** Tracks folders a user accessed, even from deleted/external media

### 5. Jump Lists & LNK Files
- **Jump Lists:** `%AppData%\Microsoft\Windows\Recent\AutomaticDestinations\`
- **LNK Files:** `%AppData%\Microsoft\Windows\Recent\`
- **Value:** Recently opened files, including from USB or network shares

### 6. Browser Artifacts
| Browser | History Location |
|---------|----------------|
| Chrome | `%LocalAppData%\Google\Chrome\User Data\Default\History` |
| Firefox | `%AppData%\Mozilla\Firefox\Profiles\*.default\places.sqlite` |
| Edge | `%LocalAppData%\Microsoft\Edge\User Data\Default\History` |

### 7. Scheduled Tasks
- **Path:** `%SystemRoot%\System32\Tasks\`
- **Value:** Persistence, automated execution

### 8. Shadow Copies
```powershell
vssadmin list shadows
# Check if attacker deleted them:
Get-WinEvent -LogName Security | Where-Object {$_.Id -eq 4688} | Where-Object {$_.Message -like "*vssadmin*delete*"}
```

### 9. Amcache & Shimcache
- **Amcache:** `%SystemRoot%\AppCompat\Programs\Amcache.hve` — Execution history
- **Shimcache (AppCompat):** `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`

### 10. Memory Artifacts (Live System)
```
# Dump with WinPmem or DumpIt
winpmem.exe memdump.raw
# Then analyse with Volatility
```

---

## Quick Triage Commands (PowerShell)

```powershell
# Active network connections
netstat -anob

# Running processes with paths
Get-Process | Select-Object Name, Id, Path

# Recently modified files (last 24h)
Get-ChildItem C:\ -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.LastWriteTime -gt (Get-Date).AddHours(-24)}

# Scheduled tasks
Get-ScheduledTask | Where-Object {$_.State -ne 'Disabled'} | Select-Object TaskName, TaskPath

# Local users and last logon
Get-LocalUser | Select-Object Name, Enabled, LastLogon

# Startup items
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location
```
