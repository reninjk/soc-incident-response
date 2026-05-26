# Runbook: Host Isolation

**Trigger:** P1/P2 incident requiring immediate endpoint containment
**Owner:** SOC Analyst (L2) / Incident Commander
**Estimated Time:** 5–15 minutes
**MITRE ATT&CK:** T1486 (Ransomware), T1059 (Command Execution), T1055 (Process Injection)

---

## Prerequisites

- [ ] EDR console access (admin role)
- [ ] Approval from SOC Manager or Incident Commander
- [ ] Incident ticket created and referenced
- [ ] Affected hostname / asset tag confirmed

---

## ⚠️ Before You Isolate

> Isolation cuts ALL network connectivity. Confirm these first:

| Check | Why |
|-------|-----|
| Is this a domain controller? | Isolating DCs can break auth for entire domain |
| Is this a critical production server? | Business impact — notify change manager |
| Is evidence collection complete? | Network isolation may stop log forwarding |
| Is a forensic memory image needed first? | Capture before isolation if possible |
| Is this a remote worker's only device? | Coordinate alternative access |

---

## Step 1 — Identify & Confirm the Host

```bash
# Confirm hostname matches affected asset
# Check in EDR console: search by hostname, IP, or username

Hostname:     ___________________________
IP Address:   ___________________________
Username:     ___________________________
Asset Owner:  ___________________________
Incident ID:  ___________________________
```

---

## Step 2 — Isolate via EDR

### CrowdStrike Falcon
```
1. Navigate to: Endpoint Security → Host Management
2. Search for hostname
3. Click host → Actions → Network Containment
4. Select "Contain" — confirm the action
5. Verify status changes to "Contained"
6. Note containment time in incident ticket
```

### Microsoft Defender for Endpoint
```
1. Navigate to: Security.microsoft.com → Devices
2. Search for device name
3. Click device → Actions → Isolate device
4. Enter isolation reason (reference incident ID)
5. Confirm → verify status shows "Isolated"
6. Note isolation time in incident ticket
```

### SentinelOne
```
1. Navigate to: Sentinels → search hostname
2. Select agent → Actions → Disconnect from Network
3. Confirm action
4. Verify network status changes to "Disconnected"
5. Note time in incident ticket
```

### Manual Network Isolation (no EDR)
```bash
# Option 1: Disable NIC via PowerShell (run as admin on host)
Get-NetAdapter | Disable-NetAdapter -Confirm:$false

# Option 2: Physical isolation
# - Unplug network cable
# - Disable Wi-Fi via hardware switch
# - Notify on-site staff to physically disconnect

# Option 3: Switch port shutdown (coordinate with network team)
# interface GigabitEthernet X/X
# shutdown
```

---

## Step 3 — Verify Isolation

```
□ EDR console confirms "Contained" / "Isolated" status
□ Ping test from SOC workstation to host fails (expected)
□ User/owner confirms device lost network access
□ Confirm EDR agent is still communicating (isolated hosts
  maintain EDR channel in most platforms)
```

---

## Step 4 — Document

```
□ Record in incident ticket:
   - Isolation method used
   - Time of isolation: ___________
   - Authorised by: ___________
   - Business impact assessment: ___________
   - Estimated duration of isolation: ___________

□ Notify asset owner / user's manager
□ Notify IT operations if server-class asset
□ Create change record if required by change management policy
```

---

## Step 5 — During Isolation (Investigation Phase)

```
□ Forensic memory capture (if not done pre-isolation)
□ Forensic disk image (coordinate with forensics team)
□ Pull EDR telemetry: process tree, file events, network events
□ Export relevant SIEM logs for the host
□ Run live response commands via EDR if supported:
```

### EDR Live Response Commands (CrowdStrike/MDE examples)
```powershell
# List running processes
Get-Process | Select-Object Name, Id, Path | Sort-Object Name

# List network connections at time of isolation
Get-NetTCPConnection | Where-Object State -eq "Established"

# List scheduled tasks
Get-ScheduledTask | Where-Object State -ne "Disabled"

# List recently modified files in temp
Get-ChildItem -Path $env:TEMP -Recurse | Sort-Object LastWriteTime -Descending | Select-Object -First 50

# List services set to auto-start
Get-Service | Where-Object StartType -eq "Automatic"
```

---

## Step 6 — De-Isolation

> Only de-isolate with explicit SOC Manager / Incident Commander approval.

```
□ Eradication confirmed complete
□ Re-image or malware removal verified
□ Patching applied if vulnerability was exploited
□ Host re-enrolled in EDR with clean status
□ SOC Manager approval obtained
□ De-isolate via EDR console (reverse of Step 2)
□ Monitor host for 24–48 hours post-release
□ Document de-isolation time in incident ticket
```

---

## Escalation

| Situation | Action |
|-----------|--------|
| EDR console unavailable | Contact EDR vendor support; use manual isolation |
| Host is a DC / critical server | Escalate to SOC Manager + IT Director before isolating |
| User is C-suite | Notify CISO before isolating |
| Host is overseas / remote site | Coordinate with local IT or facilities |

---

*Related: [Evidence Collection](evidence-collection.md) | [Ransomware Playbook](../playbooks/ransomware-response.md) | [Alert Triage](alert-triage.md)*
