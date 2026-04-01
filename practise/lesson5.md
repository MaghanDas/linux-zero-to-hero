# 1. List all usernames on the system
cat /etc/passwd | cut -d: -f1

# 2. Count how many processes are running
ps aux | wc -l

# 3. Find the 3 largest files in /var/log
du -sh /var/log/* 2>/dev/null | sort -rh | head -3

# 4. Show unique shells used on this system
cat /etc/passwd | cut -d: -f7 | sort | uniq

# 5. Find all lines in syslog from last hour with "error" (case insensitive)
grep -i "error" /var/log/syslog 2>/dev/null | tail -20

# 6. One-liner to show top 5 processes by CPU with clean output
ps aux --sort=-%cpu | awk 'NR<=6{print $1, $2, $3"%", $11}'
```

---
