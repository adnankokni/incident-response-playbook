# 🔵 Data Exfiltration Incident Response Playbook

**Severity:** Critical
**MITRE ATT&CK:** T1041 (Exfil Over C2 Channel), T1048 (Exfil Over Alt Protocol)
**Last Updated:** 2025

---

## Phase 2 — Detection & Analysis

### IOCs
- Unusually large outbound data transfers
- Connections to cloud storage (Mega, Dropbox) from servers
- DNS tunnelling (large TXT record queries)
- Data transfer at unusual hours
- Compression tools (7zip, tar) used on sensitive directories

### Detection Commands
```bash
# Monitor outbound traffic volume by destination IP
sudo tcpdump -i eth0 -n 'tcp and (dst port 443 or dst port 80)' | \
  awk '{print $5}' | cut -d. -f1-4 | sort | uniq -c | sort -rn

# Check for large file transfers in logs
sudo grep -E "bytes_sent=[0-9]{6,}" /var/log/nginx/access.log

# Detect DNS tunnelling (unusually large DNS queries)
sudo tcpdump -i eth0 port 53 -w dns_capture.pcap
# Analyse with Wireshark — look for TXT records > 100 bytes

# Check for compression activity on sensitive dirs
sudo find /home /var /opt -name "*.zip" -o -name "*.tar.gz" \
  -newer /tmp/reference_time 2>/dev/null
```

---

## Phase 3 — Containment

```bash
# Block outbound traffic to suspicious IPs
sudo iptables -A OUTPUT -d SUSPICIOUS_IP -j DROP

# Block specific ports used for exfiltration
sudo iptables -A OUTPUT -p tcp --dport 8080 -j DROP

# Capture ongoing traffic for forensics
sudo tcpdump -i eth0 -w /evidence/capture_$(date +%Y%m%d_%H%M%S).pcap
```

### Checklist
- [ ] Exfiltration traffic stopped at firewall
- [ ] Scope of data exfiltrated identified
- [ ] Legal/Compliance team notified
- [ ] Forensic copy of affected system taken
- [ ] Regulatory notification timeline assessed (GDPR: 72 hours)

---

## Phase 6 — Lessons Learned

- **Data Classification of Exfiltrated Data:**
- **Volume of Data Exfiltrated:**
- **Regulatory Notification Required (Y/N):**
- **Dwell Time Before Detection:**
- **Exfiltration Method:**
- **Recommendations:** DLP controls, egress filtering, UEBA rules
