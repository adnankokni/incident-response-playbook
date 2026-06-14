# 🔴 Ransomware Incident Response Playbook

**Severity:** Critical
**MITRE ATT&CK:** T1486 (Data Encrypted for Impact), T1490 (Inhibit System Recovery)
**Last Updated:** 2025

---

## Phase 1 — Preparation

### Required Tools
- EDR solution (CrowdStrike / Wazuh)
- Network monitoring (Wireshark / Zeek)
- Backup system access credentials
- Incident communication channel (Slack/Teams)

### Key Contacts
| Role | Responsibility |
|------|---------------|
| SOC Analyst (L1) | Initial triage and alert escalation |
| SOC Analyst (L2) | Deep investigation and containment |
| IR Lead | Coordinate response and decisions |
| Legal/Compliance | Regulatory notification decisions |
| Management | Business impact decisions |

---

## Phase 2 — Detection & Analysis

### Indicators of Compromise (IOCs)
- Files with extensions: `.locked`, `.encrypted`, `.ransom`, `.crypt`
- Ransom note files: `READ_ME.txt`, `DECRYPT_FILES.html`
- Mass file modification events in short time window
- Shadow copy deletion: `vssadmin delete shadows`
- Registry modifications for persistence
- Unusual outbound connections to C2 servers

### Severity Classification
| Severity | Criteria |
|----------|---------|
| **Critical** | Production systems encrypted, backups affected |
| **High** | Single department affected, backups intact |
| **Medium** | Isolated endpoint, no lateral movement detected |

### Triage Steps
1. Confirm alert is not a false positive
2. Identify affected systems and scope
3. Check if backups are intact and unaffected
4. Determine ransomware family if possible
5. Document initial findings with timestamps

---

## Phase 3 — Containment

### Immediate Actions (within 15 minutes)
```bash
# Isolate affected host from network immediately
# On Windows — disable network adapter
netsh interface set interface "Ethernet" admin=disabled

# Block malicious IP at firewall level
netsh advfirewall firewall add rule name="Block C2" `
  dir=out action=block remoteip=MALICIOUS_IP

# On Linux — isolate host
sudo iptables -I INPUT -j DROP
sudo iptables -I OUTPUT -j DROP
sudo iptables -I INPUT -s MANAGEMENT_IP -j ACCEPT
```

### Containment Checklist
- [ ] Affected system isolated from network
- [ ] Neighbouring systems checked for infection
- [ ] Backup systems confirmed unaffected and offline
- [ ] C2 IPs blocked at perimeter firewall
- [ ] Affected user accounts disabled
- [ ] Incident ticket created with timeline

---

## Phase 4 — Eradication

```bash
# Identify ransomware process (Windows)
tasklist /v | findstr /i "suspicious_process"

# Kill malicious process
taskkill /PID <PID> /F

# Remove persistence registry keys
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v MaliciousKey /f

# Scan with Windows Defender
"%ProgramFiles%\Windows Defender\MpCmdRun.exe" -Scan -ScanType 2
```

- Reimage affected systems from clean baseline
- Patch the vulnerability used for initial access
- Reset all credentials on affected systems

---

## Phase 5 — Recovery

```bash
# Restore from last known clean backup
# Verify backup integrity before restoring
md5sum backup_file.tar.gz
# Compare against original checksum

# Monitor for reinfection after restoration
sudo tail -f /var/log/syslog | grep -i "encrypt\|ransom\|locked"
```

- Restore systems from verified clean backups
- Monitor restored systems for 72 hours
- Gradually reconnect to network with monitoring
- Confirm business operations restored

---

## Phase 6 — Lessons Learned

### Post-Incident Report Template
- **Date/Time of Detection:**
- **Date/Time of Containment:**
- **Systems Affected:**
- **Initial Access Vector:**
- **Ransomware Family:**
- **Data Impact:**
- **Recovery Time:**
- **Root Cause:**
- **Recommendations:**

### Key Questions
1. How did the ransomware gain initial access?
2. Were backups sufficient and unaffected?
3. How long was dwell time before detection?
4. What can be improved in detection rules?
