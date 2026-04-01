# 📘 LESSON 5 — Text Processing (The Linux Superpower)

---

# grep - search through texts

```bash
grep "error" /var/log/syslog          # Find lines with "error"
grep -i "error" /var/log/syslog       # Case insensitive
grep -n "error" /var/log/syslog       # Show line numbers
grep -r "password" /etc/              # Recursive search in directory
grep -v "error" file.txt              # Show lines WITHOUT "error"
grep -c "error" file.txt              # COUNT matching lines
grep -E "error|warn|crit" file.txt    # Multiple patterns (regex)
Real world: find failed SSH logins
grep "Failed password" /var/log/auth.log
# grep - search through texts
```

# Pipes | — Chain Commands Together
```bash
The pipe | sends output of one command as input to the next. This is Linux magic.
ps aux | grep nginx              # Filter process list
cat /etc/passwd | wc -l          # Count users
ls -la | sort -k5 -n             # Sort files by size
df -h | grep -v tmpfs            # Disk usage, hide temp filesystems
dmesg | tail -20                 # Last 20 kernel messages
history | grep "git"             # Find git commands you've used
```
# awk — Column-Based Text Processing
```bash
awk '{print $1}' file.txt        # Print first column
awk '{print $1, $3}' file.txt    # Print columns 1 and 3
awk -F: '{print $1}' /etc/passwd # Use : as delimiter, print usernames
awk '{sum += $1} END {print sum}' numbers.txt  # Sum a column
Real world: show process name and PID only
ps aux | awk '{print $1, $2, $11}'
```
# sed - stream editor(find & replace)
```bash
sed 's/old/new/' file.txt         # Replace first occurrence per line
sed 's/old/new/g' file.txt        # Replace ALL occurrences
sed -i 's/old/new/g' file.txt     # Edit file IN PLACE (careful!)
sed -n '5,10p' file.txt           # Print lines 5 to 10
sed '/^#/d' file.txt              # Delete comment lines
Real world: update a config value
sed -i 's/port=8080/port=80/g' /etc/myapp/config.conf
```
# other essentail text tools
```bash
wc -l file.txt          # Count lines
wc -w file.txt          # Count words
sort file.txt           # Sort alphabetically
sort -n numbers.txt     # Sort numerically
sort -r file.txt        # Reverse sort
uniq file.txt           # Remove consecutive duplicates
sort file.txt | uniq    # Remove ALL duplicates
cut -d: -f1 /etc/passwd # Cut field 1 using : delimiter
tr 'a-z' 'A-Z'          # Translate lowercase to uppercase
```
