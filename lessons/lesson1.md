 
 # WHAT IS LINUX & HOW DOES IT WORK ?

 # BIG PICTURE
 Hardware (CPU, RAM, Disk)
        ↓
    [ KERNEL ]         ← Linux is technically just this
        ↓
  System Libraries
        ↓
   Shell (bash/zsh)    ← What you type commands into
        ↓
    Applications

# LINUX:
linux is an open-source kernal written by LINUS TORVALDS in 1991.
A disro(distribution) = linux kernal + package manager + tools + desktop(optional)

Distro      Used For
Ubuntu      Beginners, servers, cloud
CentOS/RHEL Enterprise servers
Debian      Stability-focused servers
Kali        Security/pentesting
Arch        Advanced/customization


pwd          # Print Working Directory — where am I right now?
ls           # List files
ls -la       # List ALL files including hidden, with details
cd /         # Go to root directory
cd ~         # Go to your home directory
cd ..        # Go one level up
cd -         # Go back to previous directory
clear        # Clear the screen (or Ctrl+L)
```

## Understanding the File System — Critical!
```
/               ← Root, the top of everything
├── bin/        ← Essential binaries (ls, cp, mv)
├── etc/        ← ALL configuration files live here
├── home/       ← User home directories (/home/alice)
├── var/        ← Variable data: logs, databases
├── tmp/        ← Temporary files (cleared on reboot)
├── usr/        ← User programs and libraries
├── opt/        ← Optional/third-party software
├── proc/       ← Virtual FS — kernel/process info
├── dev/        ← Device files (disks, terminals)
├── sys/        ← Another virtual FS for kernel info
└── root/       ← Home directory of root user


