# 🔥 Incident Response Playbook — SOC Analyst Portfolio

![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)
![Framework](https://img.shields.io/badge/Framework-NIST_IR-blue?style=flat-square)
![MITRE](https://img.shields.io/badge/MITRE-ATT%26CK_Mapped-red?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

A professional Incident Response Playbook collection built for SOC analyst practice
and portfolio demonstration. Covers 5 real-world attack scenarios following the
**NIST SP 800-61** IR framework with MITRE ATT&CK mapping.

---

## 🎯 What This Project Covers

This repository contains step-by-step IR procedures an analyst would follow
during a real security incident, including detection commands, containment
steps, eradication procedures, and post-incident documentation templates.

---

## 📚 Playbooks

| Playbook | Severity | MITRE Techniques | Status |
|----------|---------|-----------------|--------|
| [🔴 Ransomware](playbooks/ransomware.md) | Critical | T1486, T1490 | ✅ Complete |
| [🟠 Phishing](playbooks/phishing.md) | High | T1566, T1078 | ✅ Complete |
| [🟡 Brute Force](playbooks/brute-force.md) | Medium | T1110 | ✅ Complete |
| [🟣 Malware Infection](playbooks/malware.md) | High | T1059, T1055, T1547 | ✅ Complete |
| [🔵 Data Exfiltration](playbooks/data-exfiltration.md) | Critical | T1041, T1048 | ✅ Complete |

---

## 🏗️ IR Framework Used — NIST SP 800-61

```
┌─────────────────────────────────────────────────────────────┐
│                  INCIDENT RESPONSE LIFECYCLE                │
│                                                             │
│  1. PREPARE  →  2. DETECT  →  3. CONTAIN  →  4. ERADICATE  │
│                                     ↑               ↓       │
│                              6. LESSONS      5. RECOVER     │
│                                 LEARNED                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```
incident-response-playbook/
├── playbooks/                  # IR playbook per attack type
│   ├── ransomware.md
│   ├── phishing.md
│   ├── brute-force.md
│   ├── malware.md
│   └── data-exfiltration.md
├── templates/                  # Reusable IR documentation
│   ├── incident-report-template.md
│   └── ioc-tracking-template.md
├── scripts/                    # IR automation scripts
│   ├── collect-iocs.sh
│   └── isolate-host.sh
├── docs/                       # Supporting documentation
│   └── ir-lifecycle.md
└── README.md
```

---

## ⚙️ Tools & Technologies Referenced

| Tool | Purpose |
|------|---------|
| Wazuh SIEM | Alert detection and log analysis |
| Wireshark / tcpdump | Network traffic capture |
| iptables / UFW | Host-based firewall for containment |
| fail2ban | Automated brute force blocking |
| ClamAV | Open source malware scanning |
| VirusTotal | File hash and URL reputation |
| Microsoft Defender | Windows EDR |

---

## 🧠 Skills Demonstrated

- NIST SP 800-61 incident response framework
- MITRE ATT&CK technique mapping
- Log analysis and IOC extraction
- Host isolation and network containment
- Malware triage and analysis
- Post-incident reporting and documentation
- Bash scripting for IR automation
- Email header analysis and phishing investigation

---

## 📋 Templates Included

- **Incident Report Template** — full post-incident report structure
- **IOC Tracking Sheet** — structured IOC documentation
- **IR Automation Scripts** — bash scripts for rapid triage

---

## 🔗 Related Projects

- [Wazuh SOC Monitoring Lab](https://github.com/YOUR_USERNAME/wazuh-soc-monitoring-lab)

---

## 📄 References

- [NIST SP 800-61 Rev 2](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
- [MITRE ATT&CK Framework](https://attack.mitre.org)
- [SANS IR Process](https://www.sans.org/white-papers/incident-handlers-handbook/)

---

*Built as part of my SOC Analyst portfolio. Open to feedback and contributions.*
