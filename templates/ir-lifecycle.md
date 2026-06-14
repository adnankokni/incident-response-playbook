#!/bin/bash
# ============================================================
# collect-iocs.sh — Rapid IOC Collection for IR Triage
# Usage: sudo bash collect-iocs.sh
# Author: Your Name | SOC Analyst Portfolio
# ============================================================

OUTPUT_DIR="./ioc-collection-$(date +%Y%m%d_%H%M%S)"
mkdir -p "$OUTPUT_DIR"

echo "[*] Starting IOC collection — $(date)"
echo "[*] Output directory: $OUTPUT_DIR"

# Network connections
echo "[*] Collecting active network connections..."
netstat -tulpn > "$OUTPUT_DIR/network_connections.txt"
ss -tulpn >> "$OUTPUT_DIR/network_connections.txt"

# Running processes
echo "[*] Collecting running processes..."
ps aux > "$OUTPUT_DIR/processes.txt"

# Listening ports
echo "[*] Collecting listening services..."
ss -lntp > "$OUTPUT_DIR/listening_ports.txt"

# Logged in users
echo "[*] Collecting logged in users..."
who > "$OUTPUT_DIR/logged_users.txt"
last -n 20 >> "$OUTPUT_DIR/logged_users.txt"

# Crontabs
echo "[*] Collecting scheduled tasks..."
crontab -l > "$OUTPUT_DIR/crontab_current_user.txt" 2>/dev/null
ls -la /etc/cron* >> "$OUTPUT_DIR/crontab_current_user.txt" 2>/dev/null

# Recently modified files
echo "[*] Finding recently modified files (last 24h)..."
find /home /tmp /var/tmp -mtime -1 -type f 2>/dev/null > \
  "$OUTPUT_DIR/recent_files.txt"

# Auth log (failed logins)
echo "[*] Collecting auth log..."
sudo grep "Failed\|Accepted\|Invalid" /var/log/auth.log 2>/dev/null | \
  tail -100 > "$OUTPUT_DIR/auth_log.txt"

echo ""
echo "[+] IOC collection complete!"
echo "[+] Files saved to: $OUTPUT_DIR"
echo "[+] Review each file for indicators of compromise."
