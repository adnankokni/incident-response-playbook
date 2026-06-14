# Incident Report Template

**Incident ID:** IR-YYYY-001
**Date Created:** YYYY-MM-DD
**Analyst:** Your Name
**Status:** [ ] Open  [ ] Investigating  [ ] Resolved  [ ] Closed

---

## Executive Summary
*(2–3 sentences: what happened, impact, and current status)*

---

## Incident Timeline

| Date/Time (UTC) | Event | Source |
|----------------|-------|--------|
| YYYY-MM-DD HH:MM | Initial alert triggered | Wazuh SIEM |
| YYYY-MM-DD HH:MM | Analyst assigned | SOC Ticket System |
| YYYY-MM-DD HH:MM | Containment actioned | Analyst |
| YYYY-MM-DD HH:MM | Incident resolved | IR Team |

---

## Affected Systems

| Hostname | IP Address | OS | Role | Impact |
|---------|-----------|-----|------|--------|
| hostname-01 | 192.168.X.X | Ubuntu 22.04 | Web Server | Compromised |

---

## Indicators of Compromise (IOCs)

| Type | Value | Context |
|------|-------|---------|
| IP Address | X.X.X.X | Attacking source IP |
| File Hash (SHA256) | abc123... | Malicious file dropped |
| Domain | malicious.com | C2 domain |
| Email | attacker@domain.com | Phishing sender |

---

## Root Cause Analysis
*(What was the initial access vector? What vulnerability or weakness was exploited?)*

---

## Actions Taken
1.
2.
3.

---

## Recommendations
1.
2.
3.

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|-----|
| Initial Access | Phishing | T1566 |
| Execution | PowerShell | T1059.001 |

---

**Report Author:** 
**Report Date:**
**Reviewed By:**
