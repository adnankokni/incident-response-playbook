# 🟡 Brute Force Attack Incident Response Playbook

**Severity:** Medium–High
**MITRE ATT&CK:** T1110 (Brute Force), T1078 (Valid Accounts)
**Last Updated:** 2025

---

## Phase 2 — Detection & Analysis

### IOCs
- 5+ failed login attempts from same IP in short window
- Login attempts at unusual hours (2am–5am)
- Logins from foreign countries or Tor exit nodes
- Sequential username attempts (admin, administrator, root)
- Password spray pattern: same password across many accounts

### Detection Commands
```bash
# Check failed SSH logins on Linux
sudo grep "Failed password" /var/log/auth.log | \
  awk '{print $11}' | sort | uniq -c | sort -rn | head -20

# Count failed logins per IP
sudo grep "Failed password" /var/log/auth.log | \
  grep -oP '(\d+\.){3}\d+' | sort | uniq -c | sort -rn

# Check successful logins after failures (compromise indicator)
sudo grep "Accepted password" /var/log/auth.log | tail -20

# Check Windows Event Logs (Event ID 4625 = failed login)
Get-EventLog -LogName Security -InstanceId 4625 -Newest 50 | `
  Select TimeGenerated, Message | Format-List
```

---

## Phase 3 — Containment

```bash
# Block attacking IP immediately using iptables
sudo iptables -A INPUT -s ATTACKING_IP -j DROP
sudo iptables-save | sudo tee /etc/iptables/rules.v4

# Or use UFW (simpler)
sudo ufw deny from ATTACKING_IP to any
sudo ufw reload

# Lock targeted accounts temporarily
sudo passwd -l username

# Force SSH key authentication only (disable password auth)
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' \
  /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### Checklist
- [ ] Attacking IP blocked at firewall
- [ ] Targeted accounts locked
- [ ] Successful logins during attack window reviewed
- [ ] Geo-blocking implemented if applicable
- [ ] Account lockout policy verified/tightened

---

## Phase 4 — Eradication

```bash
# Install and configure fail2ban to auto-block future attempts
sudo apt install fail2ban -y
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

# Key settings to configure:
# bantime = 3600   (ban for 1 hour)
# findtime = 600   (within 10 minutes)
# maxretry = 5     (after 5 failures)

sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo fail2ban-client status sshd
```

---

## Phase 5 — Recovery

- Enforce MFA on all external-facing accounts
- Review and tighten account lockout policies
- Implement CAPTCHA on login pages
- Update detection rules to lower threshold
