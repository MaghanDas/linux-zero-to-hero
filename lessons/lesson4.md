
# lESSON : 4 - PROCESSES & SYSTEM INFORMATION 


# What is a Process ?
Every program running on Linux is a process. Every process has:

A PID (Process ID) — unique number
A PPID (Parent Process ID) — who spawned it
An owner — which user is running it
A state — running, sleeping, zombie, stopped


Your Shell (bash) PID=1001
        ↓ spawns
    ls command PID=1002   ← child process
        ↓ finishes
    process dies


# VIEWING PROCESSES:
ps                    # Show YOUR processes in current shell
ps aux                # Show ALL processes from ALL users
ps aux | grep nginx   # Find a specific process

<!-- # BREAKDOWN of ps aux output:
# USER   PID  %CPU %MEM   VSZ  RSS  STAT  COMMAND
# root     1   0.0  0.1  1234  567   Ss   /sbin/init
#                              ↑
#                         STAT codes:
#                         S = sleeping
#                         R = running
#                         Z = zombie (dead but not cleaned up)
#                         s = session leader
#                         + = foreground process -->


# The top and htop Commands — Real-Time Monitoring
top       # Built-in real-time process viewer
htop      # Better version (install if not present)
```

**Inside `top` — key shortcuts:**
```
P   → sort by CPU usage
M   → sort by Memory usage
k   → kill a process (enter PID)
q   → quit
1   → show individual CPU cores
```

**Reading the top header:**
```
top - 10:32:01 up 3 days,  2:15,  2 users,  load average: 0.12, 0.08, 0.05
                                                             ↑1min ↑5min ↑15min
# Load average: number of processes waiting for CPU
# On a 4-core CPU, load of 4.0 = 100% busy. Above 4.0 = overloaded


# 💼 Interview question: "Server is slow, what do you check first?"
# Answer: top or htop — check load average, CPU%, MEM%, identify the hungry process



# KILLING PROCESSES
kill PID              # Send SIGTERM (15) — polite shutdown request
kill -9 PID           # Send SIGKILL — force kill, no cleanup
kill -l               # List all signals

pkill nginx           # Kill by process name
killall nginx         # Kill ALL processes named nginx

# Common signals:
# SIGTERM (15) = "please stop" — process can clean up
# SIGKILL  (9) = "stop NOW"   — kernel forces it, no cleanup
# SIGHUP   (1) = "reload config" — used with daemons


# Background & Foreground jobs
sleep 100 &          # Run in background (& at the end)
jobs                 # List background jobs
fg %1                # Bring job #1 to foreground
bg %1                # Resume stopped job in background
Ctrl+Z               # Suspend (pause) a foreground process
Ctrl+C               # Terminate a foreground process

# real-world-use
# Start a long backup, send it to background
tar -czf backup.tar.gz /var/www/ &

# Check it's running
jobs
ps aux | grep tar

# Check when it finishes
wait   # waits for all background jobs



# SYSTEM INFORMATION COMMANDS 
uname -a              # Full kernel info (version, arch, hostname)
uname -r              # Just kernel version
hostname              # System hostname
hostname -I           # All IP addresses of this machine

cat /etc/os-release   # Distro name and version
lsb_release -a        # Distro info (Ubuntu/Debian)

uptime                # How long running + load average
date                  # Current date and time
timedatectl           # Timezone and NTP status

# Hardware info
lscpu                 # CPU details (cores, architecture, speed)
lsmem                 # Memory info
lspci                 # PCI devices (GPU, network cards)
lsblk                 # Block devices (disks, partitions)
lsusb                 # USB devices

# Resource usage
free -h               # RAM usage (human readable)
df -h                 # Disk space usage per filesystem
df -h /               # Just root partition
du -sh /var/log/      # How much space does this folder use?
du -sh /* 2>/dev/null # Size of every root-level folder



# /proc - The Process Virtual filesystem
This is where Linux exposes kernel and process info as files.

cat /proc/cpuinfo         # Detailed CPU info
cat /proc/meminfo         # Detailed memory info
cat /proc/uptime          # Uptime in seconds
cat /proc/version         # Kernel version
ls /proc/                 # Each numbered folder = a PID
cat /proc/1/status        # Info about PID 1 (init/systemd)
cat /proc/self/status     # Info about YOUR current shell process


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