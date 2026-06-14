# 🟠 Phishing Incident Response Playbook

**Severity:** High
**MITRE ATT&CK:** T1566 (Phishing), T1078 (Valid Accounts), T1110 (Brute Force)
**Last Updated:** 2025

---

## Phase 1 — Preparation

### Detection Sources
- Email gateway alerts (Microsoft Defender for Office 365 / Proofpoint)
- User-reported suspicious emails
- SIEM correlation rules on login anomalies
- DNS filtering alerts on malicious domains

---

## Phase 2 — Detection & Analysis

### Phishing IOCs
- Sender domain spoofing (e.g. `paypa1.com` instead of `paypal.com`)
- Mismatched Reply-To address
- Urgent language: "Your account will be suspended"
- Suspicious attachments: `.docm`, `.xlsm`, `.iso`, `.lnk` files
- Embedded URLs redirecting to credential harvesting pages
- Links using URL shorteners

### Email Header Analysis Commands
```bash
# Extract email headers and analyse
# Check SPF, DKIM, DMARC records
dig TXT suspicious-domain.com | grep -E "spf|dmarc|dkim"

# Check if domain is newly registered (red flag)
whois suspicious-domain.com | grep -E "Creation Date|Registrar"

# Check URL reputation
curl -s "https://urlhaus-api.abuse.ch/v1/url/" \
  -d "url=http://suspicious-url.com"
```

---

## Phase 3 — Containment

```bash
# Block malicious sender domain at email gateway
# (Commands vary by email platform — document your steps)

# Block phishing domain in DNS
# Add to corporate DNS blocklist or firewall

# Disable compromised user account immediately
# Active Directory (Windows)
Disable-ADAccount -Identity "compromised.user"

# Force password reset
Set-ADAccountPassword -Identity "compromised.user" -Reset `
  -NewPassword (ConvertTo-SecureString "TempPass123!" -AsPlainText -Force)
```

### Containment Checklist
- [ ] Malicious email quarantined / removed from all mailboxes
- [ ] Phishing domain blocked at DNS/firewall
- [ ] Compromised accounts disabled
- [ ] MFA verified active on affected accounts
- [ ] Affected user notified and interviewed
- [ ] Similar emails searched in email gateway

---

## Phase 4 — Eradication

```bash
# Search for malicious email across all mailboxes
# Microsoft 365 — Content Search
New-ComplianceSearch -Name "PhishingSearch" `
  -ExchangeLocation All `
  -ContentMatchQuery "subject:'Urgent Action Required' AND from:suspicious@domain.com"

# Remove matching emails
New-ComplianceSearchAction -SearchName "PhishingSearch" -Purge -PurgeType HardDelete
```

---

## Phase 5 — Recovery

- Reset credentials for all affected users
- Re-enable accounts after password reset and MFA confirmation
- Conduct targeted phishing awareness training for affected users
- Update email filtering rules to catch similar campaigns

---

## Phase 6 — Lessons Learned

- **Users Affected:**
- **Credentials Compromised:**
- **Initial Access Achieved (Y/N):**
- **Dwell Time:**
- **Detection Source:**
- **Recommendations:** Update phishing simulation training, tune email filters
