# Runbook: Digital Evidence Collection

**Trigger:** Any P1/P2 incident; any incident with legal/regulatory implications
**Owner:** SOC Analyst (L2) / Forensic Lead
**IMPORTANT:** Evidence must be collected BEFORE host isolation where possible. Maintain chain of custody at all times.

---

## Legal & Chain of Custody

> ⚠️ Improperly collected evidence may be inadmissible in legal proceedings. Follow these steps precisely.

```
□ Create an evidence log BEFORE collecting anything (see template below)
□ Only authorised personnel to access/collect evidence
□ Record every person who accesses evidence (who, when, why)
□ Never work directly on original evidence — always work on copies
□ Hash all evidence files immediately after collection
□ Store evidence in access-controlled location
□ Do not discuss evidence contents outside the incident team
```

### Chain of Custody Log
| # | Item | Collected By | Date/Time | Hash (SHA256) | Storage Location | Notes |
|---|------|-------------|-----------|--------------|-----------------|-------|
| 1 | | | | | | |
| 2 | | | | | | |

---

## Priority Order of Evidence Collection

Collect in this order (most volatile → least volatile):

1. **CPU registers & cache** — captured in memory image
2. **RAM / Memory** — must be captured while system is running
3. **Running processes & network connections** — EDR live response or manual
4. **Temporary files & paging file** — on disk, but may be overwritten
5. **Disk image** — full forensic copy of storage
6. **Logs** — SIEM, EDR, firewall, proxy, email
7. **External evidence** — ISP records, cloud provider logs, physical CCTV

---

## Memory Acquisition (Live System)

### Windows — WinPmem
```powershell
# Download WinPmem to a clean USB
# Run from USB (do NOT install to suspect system)

winpmem_mini_x64_rc2.exe memory_image_HOSTNAME_DATETIME.raw

# Verify hash after acquisition
Get-FileHash memory_image_HOSTNAME_DATETIME.raw -Algorithm SHA256
```

### Linux — LiME (Linux Memory Extractor)
```bash
# Load LiME kernel module
sudo insmod lime.ko "path=/external/memory_HOSTNAME.lime format=lime"

# Hash the image
sha256sum /external/memory_HOSTNAME.lime
```

---

## Disk Imaging

### Windows — FTK Imager (GUI)
```
1. Launch FTK Imager as Administrator
2. File → Create Disk Image
3. Select source: Physical Drive
4. Image type: E01 (Expert Witness Format) — preferred for forensics
5. Set destination to external forensic drive
6. Check "Verify images after they are created"
7. Record hash values shown upon completion
```

### Linux — dd / dcfldd
```bash
# Using dcfldd (with hashing)
dcfldd if=/dev/sda of=/forensic/image_HOSTNAME_DATETIME.dd \
  hash=sha256 hashlog=/forensic/image_HOSTNAME_DATETIME.hash \
  bs=512 conv=noerror,sync

# Verify
sha256sum /forensic/image_HOSTNAME_DATETIME.dd
```

---

## Log Collection

### SIEM Logs
```
□ Define time window: [incident start - 72 hours] to [now]
□ Export raw logs for:
   - Affected host(s): all event types
   - Affected user account(s): all event types
   - Source/destination IPs: all event types
□ Export in immutable format (PDF + raw)
□ Record search queries used to extract logs
□ Hash exported log files
```

### Windows Event Logs (from host)
```powershell
# Export key event logs
wevtutil epl Security C:\Evidence\Security.evtx
wevtutil epl System C:\Evidence\System.evtx
wevtutil epl Application C:\Evidence\Application.evtx
wevtutil epl "Microsoft-Windows-PowerShell/Operational" C:\Evidence\PowerShell.evtx
wevtutil epl "Microsoft-Windows-Sysmon/Operational" C:\Evidence\Sysmon.evtx

# Hash all exported files
Get-ChildItem C:\Evidence\ | ForEach-Object { 
  $hash = (Get-FileHash $_.FullName -Algorithm SHA256).Hash
  "$hash  $($_.Name)"
} | Out-File C:\Evidence\HASHES.txt
```

### Network / Firewall Logs
```
□ Firewall: export flows for affected IPs for incident window
□ Proxy: export web requests for affected user/host
□ DNS: export DNS queries from affected host
□ VPN: export connection logs for affected user
□ Email gateway: export email headers and metadata
```

### Cloud / SaaS Logs
```
□ Microsoft 365 Unified Audit Log (UAL) — export via Compliance portal
□ Azure AD Sign-In and Audit logs — export via Azure portal
□ AWS CloudTrail — export relevant trail for affected account/role
□ Google Workspace Admin Audit — export relevant logs
```

---

## Running Process & Network State Capture

```powershell
# Capture before isolation — run these IMMEDIATELY

# Running processes with full paths
Get-Process | Select-Object Id, ProcessName, Path, Company, StartTime | 
  Export-Csv C:\Evidence\processes_$(Get-Date -Format yyyyMMdd_HHmmss).csv

# Network connections
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, 
  RemotePort, State, OwningProcess | 
  Export-Csv C:\Evidence\netconn_$(Get-Date -Format yyyyMMdd_HHmmss).csv

# Scheduled tasks
Get-ScheduledTask | Export-Csv C:\Evidence\scheduled_tasks.csv

# Autorun locations
reg export HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run C:\Evidence\autorun_hklm.reg
reg export HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run C:\Evidence\autorun_hkcu.reg

# Active users
query user > C:\Evidence\active_users.txt

# Loaded drivers
driverquery /FO CSV > C:\Evidence\drivers.csv
```

---

## Evidence Storage & Handoff

```
□ All files hashed and hash log created
□ Evidence stored in password-protected, access-controlled location
□ Evidence inventory completed (see chain of custody log above)
□ Evidence location documented in incident ticket
□ If physical media: labelled with case ID, date, collector name
□ If legal hold required: notify Legal and preserve for duration
□ Handoff to forensic analyst: update chain of custody log
```

---

## Evidence Checklist Summary

| Evidence Type | Collected | Hash Recorded | Location | Notes |
|---------------|-----------|---------------|----------|-------|
| Memory image | | | | |
| Disk image | | | | |
| Windows Event Logs | | | | |
| SIEM log export | | | | |
| Network/firewall logs | | | | |
| EDR telemetry export | | | | |
| Email logs | | | | |
| Cloud audit logs | | | | |
| Screenshots | | | | |

---

*Related: [Host Isolation](host-isolation.md) | [Incident Report Template](../templates/incident-report-template.md)*
