# IOC Tracking Sheet

**Incident ID:** IR-YYYY-001
**Last Updated:** YYYY-MM-DD

---

## IP Addresses

| IP | Type | First Seen | Last Seen | Blocked | Notes |
|----|------|-----------|----------|---------|-------|
| 1.2.3.4 | Attacker C2 | 2025-01-01 | 2025-01-02 | ✅ Yes | Blocked at firewall |

## File Hashes

| Hash Type | Value | Filename | Action |
|-----------|-------|----------|--------|
| SHA256 | abc123... | malware.exe | Quarantined |

## Domains / URLs

| Domain/URL | Type | Blocked | Notes |
|-----------|------|---------|-------|
| evil.com | C2 Domain | ✅ Yes | DNS blocked |

## Email Addresses

| Email | Type | Notes |
|-------|------|-------|
| attacker@evil.com | Phishing sender | Blocked at gateway |

## User Accounts

| Username | System | Action | Status |
|---------|--------|--------|--------|
| jsmith | AD | Password reset, MFA enforced | Resolved |
