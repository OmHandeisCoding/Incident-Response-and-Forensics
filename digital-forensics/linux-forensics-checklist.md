# Linux Forensics Checklist

## Key Artifacts to Collect

### 1. Authentication & Login Logs
| File | Location | Content |
|------|----------|---------|
| auth.log | `/var/log/auth.log` (Debian/Ubuntu) | SSH logins, sudo, su |
| secure | `/var/log/secure` (RHEL/CentOS) | SSH logins, sudo |
| wtmp | `/var/log/wtmp` | All logins (binary — read with `last`) |
| btmp | `/var/log/btmp` | Failed logins (binary — read with `lastb`) |
| utmp | `/var/run/utmp` | Currently logged-in users |
| lastlog | `/var/log/lastlog` | Last login per user |

```bash
# Useful commands
last -F                # Full login history with timestamps
lastb                  # Failed login attempts
who                    # Currently logged-in users
w                      # Who is logged in + what they're doing
```

### 2. Shell History
```bash
# User bash history
~/.bash_history
~/.zsh_history
~/.sh_history

# Check history for suspicious commands
grep -E "wget|curl|nc |ncat|chmod.*777|base64|/tmp/" ~/.bash_history

# Note: Attacker may clear history — check inode for modification time
stat ~/.bash_history
```

### 3. Cron Jobs (Persistence)
```bash
# System-wide cron
/etc/crontab
/etc/cron.d/
/etc/cron.hourly/ /etc/cron.daily/ /etc/cron.weekly/ /etc/cron.monthly/

# Per-user cron
crontab -l -u <username>
/var/spool/cron/crontabs/

# Systemd timers (modern equivalent of cron)
systemctl list-timers --all
```

### 4. Running Processes & Services
```bash
ps auxf              # Full process tree
lsof -i              # Open network connections per process
netstat -antp        # Network connections with PID/program
ss -tulnp            # Socket statistics

# Check for hidden processes (rootkit detection)
ls /proc | sort -n   # Compare with ps output
```

### 5. Startup & Persistence Mechanisms
```bash
# Systemd services
systemctl list-units --type=service --state=running
ls /etc/systemd/system/       # Custom services

# Init scripts (legacy)
ls /etc/init.d/
ls /etc/rc.local

# PAM modules (backdoor mechanism)
cat /etc/pam.d/sshd
ls /lib/security/

# SUID binaries (privilege escalation)
find / -perm -4000 -type f 2>/dev/null

# LD_PRELOAD injection
cat /etc/ld.so.preload
```

### 6. Users and Accounts
```bash
cat /etc/passwd        # All user accounts
cat /etc/shadow        # Password hashes
cat /etc/group         # Group memberships
cat /etc/sudoers       # Sudo privileges

# Accounts with UID 0 (root-equivalent — should be ONLY root)
awk -F: '$3==0 {print $1}' /etc/passwd

# Recently added users
ls -lt /home/         # Check modification times
```

### 7. /tmp and /dev/shm (Attacker Staging Areas)
```bash
ls -la /tmp/
ls -la /dev/shm/
ls -la /var/tmp/

# Find executables in temp directories (major red flag)
find /tmp /var/tmp /dev/shm -type f -executable
```

### 8. File System Timeline
```bash
# Find recently modified files (last 72 hours)
find / -mtime -3 -type f 2>/dev/null | grep -v /proc | grep -v /sys

# Find recently created files
find / -ctime -3 -type f 2>/dev/null
```

### 9. Network Configuration
```bash
ip addr show          # Network interfaces
ip route show         # Routing table
cat /etc/hosts        # Local DNS overrides (backdoor technique)
cat /etc/resolv.conf  # DNS servers
iptables -L -n        # Firewall rules
```

### 10. Log Files
```bash
/var/log/syslog       # General system log
/var/log/kern.log     # Kernel messages
/var/log/apache2/     # Web server (if applicable)
/var/log/nginx/       # Web server (if applicable)
/var/log/audit/audit.log  # Linux Audit daemon (if auditd running)
```
