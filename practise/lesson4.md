# 1. Find the top 5 CPU-hungry processes
ps aux --sort=-%cpu | head -6

# 2. Find the top 5 memory-hungry processes
ps aux --sort=-%mem | head -6

# 3. Find PID of your bash shell
echo $$         # $$ is a special var = current PID
ps aux | grep $$

# 4. Check your system resources
echo "=== CPU ===" && lscpu | grep -E "Model|Core|Thread"
echo "=== RAM ===" && free -h
echo "=== DISK ===" && df -h /
echo "=== UPTIME ===" && uptime

# 5. Background job practice
sleep 60 &
sleep 90 &
jobs            # You should see 2 jobs
kill %1         # Kill job #1
jobs            # Now only 1 job remains

# 6. Explore /proc
cat /proc/cpuinfo | grep "model name" | head -1
cat /proc/meminfo | grep MemTotal
ls /proc/ | grep -E '^[0-9]+' | wc -l   # How many processes running?
```

---

## 🔑 Cheat Sheet So Far
```
ps aux              → snapshot of all processes
top / htop          → live process monitor
kill -9 PID         → force kill
df -h               → disk space
free -h             → RAM
lscpu               → CPU info
uname -a            → kernel info
/proc/              → live kernel data as files
load average        → processes waiting for CPU