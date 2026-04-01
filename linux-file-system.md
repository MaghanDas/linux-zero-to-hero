
# 📁 Linux File System — Complete Deep Dive

# The Golden Rule First
# "In Linux, everything is a file."
# examples: 
Hardware devices → files in /dev/
Running processes → files in /proc/
System settings → files in /sys/
Network sockets → files
Directories → special files

# THE FULL TREE_ OVERVIEW
/
├── bin/
├── sbin/
├── lib/
├── lib64/
├── usr/
│   ├── bin/
│   ├── sbin/
│   ├── lib/
│   ├── local/
│   └── share/
├── etc/
├── home/
├── root/
├── var/
│   ├── log/
│   ├── spool/
│   └── cache/
├── tmp/
├── opt/
├── srv/
├── dev/
├── proc/
├── sys/
├── run/
├── boot/
├── media/
└── mnt/

# TIER:1
# / - Root Directory 
ls /
The absolute top of the entire filesystem tree
Everything lives under / — there is no higher level
Only the root user owns this
Think of it like C:\ on Windows, but everything is here

# Check root filesystem usage
df -h /
# Who owns root?
ls -la / | head -5
"What happens if / fills up?"
The system becomes unstable — can't write logs, can't create processes, SSH may fail. Always monitor / disk usage


# /bin - essential User binaries
ls /bin
```

- Contains **essential commands** needed by ALL users
- Available even in **single-user/rescue mode** (before other filesystems mount)
- No external dependencies — self-contained

**What lives here:**
```
ls, cp, mv, rm, mkdir, rmdir   → file operations
cat, echo, grep, sed, awk      → text tools
bash, sh                       → shells
chmod, chown                   → permission tools
mount, umount                  → filesystem mounting
ping, hostname                 → basic networking
gzip, tar                      → compression


💡 On modern systems (Ubuntu 20+), /bin is a symlink to /usr/bin. Run ls -la /bin to confirm.

bashls -la /bin          # See if it's a symlink
which ls             # Where does 'ls' actually live?
file /bin/ls         # What type of file is ls?


# /sbin - System Binaries(Admin only)
ls /sbin
```

- Like `/bin` but for **system administration** commands
- Typically require **root** to run
- Critical for system boot and repair

**What lives here:**
```
init / systemd         → process manager (PID 1)
fsck                   → filesystem check/repair
fdisk, parted          → disk partitioning
ip, ifconfig           → network configuration
iptables               → firewall
reboot, shutdown       → power management
useradd, userdel       → user management
swapon, swapoff        → swap management
mkfs                   → format filesystems

which reboot           # /sbin/reboot
ls -la /sbin/init      # Often a symlink to systemd

# /lib and /lib64 — Shared Libraries
ls /lib
ls /lib64
```

- Shared **library files** (.so files) — like `.dll` on Windows
- Used by programs in `/bin` and `/sbin`
- **Must be available at boot** — before `/usr` is mounted
- `/lib64` = 64-bit versions of the same libraries

**What lives here:**
```
libc.so          → C standard library (almost everything uses this)
libm.so          → Math library
libpthread.so    → Threading library
ld-linux.so      → Dynamic linker/loader
modules/         → Kernel modules (drivers!)


# See what libraries a binary needs
ldd /bin/ls
# List kernel modules
ls /lib/modules/$(uname -r)/
# See loaded kernel modules
lsmod

# What is a shared library?"
A compiled code file loaded at runtime by multiple programs simultaneously. Saves memory vs. static linking where each program carries its own copy.


# TIER-2 - The /usr Hierarchy (Unix System Resources)
/usr is the largest directory on most systems. It contains the majority of user-space programs and data.
bashdu -sh /usr    # Usually 3-10GB

# /usr/bin — Non-Essential User Programs
All the programs you use daily that aren't needed at boot
Installed by your package manager

ls /usr/bin | wc -l    # Hundreds of programs!
```

**Examples:**
```
python3, perl, ruby        → scripting languages
vim, nano, emacs           → text editors
git, curl, wget            → developer tools
ssh, scp, sftp             → remote access
top, htop, ps              → monitoring
zip, unzip, tar            → archiving
gcc, make                  → compilers
mysql, psql                → database clients
docker                     → containers
```

---

## `/usr/sbin` — Non-Essential System Admin Programs
```
apache2, nginx      → web servers
sshd                → SSH daemon
cron                → job scheduler
useradd, groupadd   → user management tools
tcpdump             → network analysis


# /usr/lib — Libraries for /usr/bin Programs
Libraries for programs in /usr/bin and /usr/sbin
Application-specific data and plugin files

ls /usr/lib/python3*/     # Python standard library
ls /usr/lib/gcc/          # GCC compiler libraries
```

---

## `/usr/local` — Locally Installed Software
```
/usr/local/
├── bin/       ← manually compiled programs go here
├── sbin/
├── lib/
├── include/
└── share/

Software you compile from source or install manually
NOT managed by apt/yum — safe from package manager overwrites
Has higher priority in $PATH than /usr/bin

# When you do: ./configure && make && make install
# Binaries go to /usr/local/bin
# Libraries go to /usr/local/lib

which python3       # Is it system or local?
echo $PATH          # /usr/local/bin appears first
```

---

## `/usr/share` — Architecture-Independent Data
```
/usr/share/
├── doc/           ← documentation for every package
├── man/           ← man pages (manual entries)
├── locale/        ← language/translation files
├── fonts/         ← system fonts
├── icons/         ← desktop icons
├── applications/  ← .desktop files (app launchers)
└── zoneinfo/      ← timezone data


# Tier-3 - Configuration & Data 

# /etc — System Configuration Files ⭐ (Most Important for Sysadmins)

# ls /etc
Every system-wide configuration file lives here
Plain text files — editable with any text editor
Changes here affect the whole system
Back this up before editing anything!

# Critical Files Inside /etc:
/etc/passwd          # User account info (NOT passwords anymore)
/etc/shadow          # Actual hashed passwords (root only)
/etc/group           # Group definitions
/etc/sudoers         # Who can use sudo and how
/etc/hostname        # System's hostname
/etc/hosts           # Local DNS — IP to hostname mapping
/etc/resolv.conf     # DNS server configuration
/etc/fstab           # Filesystems to auto-mount at boot
/etc/crontab         # System-wide scheduled tasks
/etc/profile         # System-wide shell environment
/etc/bashrc          # System-wide bash settings
/etc/environment     # System-wide environment variables
/etc/ssh/            # SSH server and client configuration
/etc/ssl/            # SSL certificates and keys
/etc/apt/            # APT package manager config (Debian/Ubuntu)
/etc/yum.conf        # YUM package manager config (RHEL/CentOS)
/etc/nginx/          # Nginx web server config
/etc/mysql/          # MySQL database config
/etc/systemd/        # Systemd service configs
/etc/network/        # Network interface configs
/etc/os-release      # Distro identification


# /etc files: 
# /etc/passwd — format:
# username:x:UID:GID:comment:home_dir:shell
cat /etc/passwd
# root:x:0:0:root:/root:/bin/bash
#  ↑   ↑  ↑  ↑    ↑       ↑         ↑
# user pwd uid gid info   home    shell
# 'x' means password is in /etc/shadow

# /etc/shadow — format (root only):
# username:hashed_password:last_change:min:max:warn:inactive:expire
sudo cat /etc/shadow | head -3

# /etc/fstab — auto-mount at boot:
cat /etc/fstab
# /dev/sda1  /        ext4   defaults   0  1
#   device  mountpoint type  options  dump fsck

# /etc/hosts — local DNS overrides:
cat /etc/hosts
# 127.0.0.1   localhost
# 192.168.1.10  myserver.local myserver

# /etc/resolv.conf — DNS servers:
cat /etc/resolv.conf
# nameserver 8.8.8.8
# nameserver 8.8.4.4

"How do you change a hostname permanently?"
bashhostnamectl set-hostname newname     # Modern way
# AND edit /etc/hostname
# AND update /etc/hosts


# /home - user home dicretories
ls /home
ls -la /home/alice/
```

- Each user gets `/home/username/`
- Contains all personal files, configs, and data
- Only that user (and root) can access it by default

**What's inside a home directory:**
```
~/.bashrc          # Personal bash config (aliases, functions)
~/.bash_profile    # Runs at login
~/.bash_history    # Every command you've typed
~/.ssh/            # SSH keys and known hosts
~/.config/         # App configuration files
~/.local/          # User-specific programs and data
~/.profile         # Shell-agnostic login config

ls -la ~           # See ALL hidden files in your home
cat ~/.bash_history | tail -20    # Your recent commands
cat ~/.bashrc                     # Your shell customizations

# /root — Root User's Home Directory
bashsudo ls -la /root

The root user's home directory — NOT /home/root
Separate from /home so root can log in even if /home is unmounted
Only accessible by roo

# /var - variable data (changes constantly)
ls /var
du -sh /var/*  2>/dev/null | sort -rh | head -10
Files here grow and change while the system runs.

# other critical subdirectories
/var/log/          # ← ALL system logs live here
/var/log/syslog        # General system messages
/var/log/auth.log      # Authentication: logins, sudo, SSH
/var/log/kern.log      # Kernel messages
/var/log/dmesg         # Boot/hardware messages
/var/log/apt/          # Package install history
/var/log/nginx/        # Web server logs
/var/log/mysql/        # Database logs
/var/log/journal/      # Systemd journal (binary format)

/var/cache/        # Cached data from applications
/var/cache/apt/    # Downloaded .deb packages cached here

/var/spool/        # Queued data waiting to be processed
/var/spool/cron/   # User crontab files
/var/spool/mail/   # Local mail queue

/var/lib/          # Persistent application state data
/var/lib/mysql/    # MySQL database files
/var/lib/docker/   # Docker images and containers
/var/lib/apt/      # APT package database

/var/run/ → /run   # PIDs and runtime state (symlink)
/var/tmp/          # Temp files preserved across reboots

# Real-world log investigation
tail -f /var/log/syslog               # Watch live system events
grep "error" /var/log/syslog -i       # Find errors
grep "Failed" /var/log/auth.log       # Failed logins
journalctl -xe                        # Systemd journal, recent errors
journalctl -u nginx --since today     # Nginx logs today

# 💼 Interview: "Where do you look first when a service crashes?"
# /var/log/ — specifically journalctl -u servicename -xe or the app's log in /var/log/appname/



# Tier 4 - Virtual & Specila filesystems 
# these don't extist on disk - the kernal genereated them in memory on the fly.

# /proc - process & kernal information 
ls /porc 
A virtual filesystem — nothing here is on disk
The kernel exposes live system data as readable files
Every running process has a folder /proc/PID/

# System-wide info
cat /proc/cpuinfo          # Detailed CPU info
cat /proc/meminfo          # Memory stats
cat /proc/uptime           # Seconds since boot
cat /proc/version          # Kernel version string
cat /proc/cmdline          # Kernel boot parameters
cat /proc/mounts           # Currently mounted filesystems
cat /proc/net/dev          # Network interface statistics
cat /proc/loadavg          # Load average (same as uptime)
cat /proc/sys/kernel/hostname    # Current hostname
cat /proc/sys/vm/swappiness      # How aggressively to use swap

# Per-process info (use real PID)
PID=$$                     # Your shell's PID
ls /proc/$PID/
cat /proc/$PID/status      # Process state, memory, etc.
cat /proc/$PID/cmdline     # What command started this process
cat /proc/$PID/environ     # Environment variables
ls -la /proc/$PID/fd/      # Open file descriptors
cat /proc/$PID/maps        # Memory map of the process

# "How can you find what files a process has open without any extra tools?"
# bashls -la /proc/PID/fd/    # Every open file is a symlink here



# /sys - System & Hardware information 
ls /sys
Also virtual — kernel exposes hardware info
More structured than /proc
Used to read AND write kernel parameters at runtime


ls /sys/class/net/          # Network interfaces
ls /sys/block/              # Block devices (disks)
cat /sys/class/net/eth0/speed           # Network speed
cat /sys/block/sda/size                 # Disk size in sectors
cat /sys/class/power_supply/BAT0/capacity  # Battery %

# You can WRITE to /sys to change kernel behavior at runtime!
echo 1 > /sys/class/leds/caps_lock/brightness   # Toggle LED

# /dev - device files
ls /dev
Every hardware device is represented as a file
Programs read/write devices like files
# Types of device files:
# b = block device (read in chunks — disks)
# c = character device (read byte by byte — terminals, serial)

ls -la /dev/sd*        # Hard drives (sda, sdb...)
ls -la /dev/nvme*      # NVMe SSDs
ls -la /dev/tty*       # Terminal devices
ls -la /dev/null       # The black hole — discard output
ls -la /dev/zero       # Infinite source of zero bytes
ls -la /dev/random     # Random number generator
ls -la /dev/urandom    # Non-blocking random
ls -la /dev/stdin      # Standard input
ls -la /dev/stdout     # Standard output

# special devices to know
# /dev/null — discard unwanted output
command 2>/dev/null         # Throw away errors
command > /dev/null 2>&1    # Throw away ALL output

# /dev/zero — fill files with zeros
dd if=/dev/zero of=test.img bs=1M count=100   # Create 100MB empty file

# /dev/random — generate random data
dd if=/dev/random bs=1 count=16 | xxd        # 16 random bytes

# Disk devices:
# /dev/sda         = first SATA disk
# /dev/sda1        = first partition of first disk
# /dev/sdb         = second SATA disk
# /dev/nvme0n1     = first NVMe disk
# /dev/nvme0n1p1   = first partition of NVMe disk
lsblk              # See all block devices cleanly

# What is /dev/null?"
# A special device file that discards everything written to it. Reading from it returns EOF. Used to suppress output in scripts.



# TIER - 5 - BOOT, MOUNT & RUNTIME 

# /boot - Boot Files: 
ls -la /boot
Everything needed to boot the system
Critical — if this is corrupted, system won't start

/boot/vmlinuz-*       # The actual compressed Linux kernel
/boot/initrd.img-*    # Initial RAM disk — temporary root FS used at boot
/boot/grub/           # GRUB bootloader files
/boot/grub/grub.cfg   # GRUB configuration (don't edit manually)
/boot/System.map-*    # Kernel symbol table (for debugging)
/boot/config-*        # Kernel build configuration

ls /boot/
uname -r                      # Running kernel version
ls /boot/vmlinuz*             # All installed kernels
cat /boot/grub/grub.cfg | grep menuentry   # Boot menu entries


# /tmp - temporary files 
ls /tmp

Temporary files created by programs and users
Cleared on every reboot (sometimes in-memory with tmpfs)
World-writable — any user can write here
Never store important data here

df -h /tmp              # Often mounted as tmpfs (RAM)
ls -la /tmp             # Sticky bit set on /tmp (t in permissions)
stat /tmp               # See permissions: drwxrwxrwt (the 't' = sticky bit)


# 💼 Interview: "What is the sticky bit on /tmp?"
# Even though /tmp is world-writable, the sticky bit means users can only delete their own files — not files created by others.


# /run - Runtime data
ls /run
Stores runtime state since last boot
Replaces the old /var/run
Lives in RAM (tmpfs) — cleared on reboot

/run/sshd.pid         # PID file of SSH daemon
/run/lock/            # Lock files
/run/user/1000/       # Runtime dir for user with UID 1000
/run/systemd/         # Systemd runtime data

cat /run/sshd.pid                      # PID of running SSH server
ls /run/user/$(id -u)/                 # Your user's runtime files

# /opt — Optional/Third-Party Software
ls /opt

Self-contained third-party applications
Not managed by system package manager
Each app lives in its own subdirectory

# Common things in /opt:
/opt/google/chrome/      # Google Chrome
/opt/java/               # Manually installed Java
/opt/mycompany/app/      # Your company's custom software
/opt/atlassian/          # Atlassian tools (Jira, Confluence)

ls /opt/

💡 If a vendor says "install to /opt", it means they want their software isolated from system files.


# /srv — Service Data
ls /srv

Data served by this system
Web server files, FTP files, etc.

/srv/http/        # Web server files (some distros)
/srv/ftp/         # FTP server files
/srv/git/         # Git repositories

/media and /mnt — Mount Points
ls /media
ls /mnt
/media/           # Auto-mounted removable media
/media/username/  # USB drives, CD-ROMs auto-mount here

/mnt/             # Temporary manual mounts
                  # Admin mounts things here manually

# Example: mount a USB drive manually
mount /dev/sdb1 /mnt/usb
ls /mnt/usb
umount /mnt/usb

# Example: mount a network share
mount -t nfs 192.168.1.5:/share /mnt/nfs


<!-- 📊 Complete Reference Table
DirectoryContainsEditable?On Disk?/binEssential user commandsNoYes/sbinEssential admin commandsNoYes/libEssential libraries + kernel modulesNoYes/usr/binMost user programsNoYes/usr/localManually installed softwareYesYes/etcALL config filesYesYes/homeUser personal filesYesYes/rootRoot's homeYesYes/var/logSystem & app logsNo (auto)Yes/var/libApp persistent stateNo (auto)Yes/tmpTemporary filesYesRAM/Disk/runRuntime PIDs, socketsNo (auto)RAM/procLive kernel/process dataSomeRAM only/sysHardware/kernel paramsSomeRAM only/devDevice filesNoRAM only/bootKernel & bootloaderCarefullyYes/optOptional softwareYesYes/mntManual mount pointsYesDepends/mediaAuto-mount removableAutoDepends
 -->


# Practise - sesson
# 1. Explore the whole tree
find / -maxdepth 1 -type d 2>/dev/null

# 2. Find the biggest directories on your system
du -sh /* 2>/dev/null | sort -rh | head -10

# 3. Investigate /proc deeply
cat /proc/cpuinfo | grep -c "processor"   # How many CPU cores?
cat /proc/meminfo | grep "MemFree"        # Free RAM in kB
cat /proc/1/cmdline                        # What is PID 1?

# 4. Explore device files
ls -la /dev/sd* 2>/dev/null || ls -la /dev/nvme* 2>/dev/null
ls -la /dev/null /dev/zero /dev/random

# 5. Read your own config files
cat /etc/hostname
cat /etc/hosts
cat /etc/os-release
cat /etc/fstab

# 6. Check log files
sudo tail -20 /var/log/syslog
sudo tail -20 /var/log/auth.log

# 7. See what's mounted
cat /proc/mounts
mount | column -t

# 8. Explore your home hidden files
ls -la ~
cat ~/.bashrc

# 💼 Top Interview Questions on Filesystem:

1. Where are passwords stored? → /etc/shadow (hashed)
2. Where are cron jobs stored? → /var/spool/cron/ and /etc/cron.d/
3. Where are logs? → /var/log/
4. Difference between /bin and /usr/bin? → /bin needed at boot, /usr/bin not
5. What is /proc? → Virtual filesystem, kernel exposes live data as files
6. Where to install custom software? → /usr/local/ or /opt/
7. What is /dev/null? → Data black hole, discards everything written to it
8. What does /etc/fstab do? → Defines filesystems to mount automatically at boot

