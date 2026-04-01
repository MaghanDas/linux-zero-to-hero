 
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

---

## 📌 What Is Linux?

- **Linux** is an **open-source kernel** created by **Linus Torvalds in 1991**.
- A **distribution (distro)** is:

> **Linux kernel + package manager + tools + (optional) desktop environment**

---

## 🧩 Popular Linux Distributions

| Distro        | Used For                         |
|--------------|---------------------------------|
| Ubuntu       | Beginners, servers, cloud       |
| CentOS / RHEL| Enterprise servers              |
| Debian       | Stability-focused systems       |
| Kali Linux   | Security / pentesting           |
| Arch Linux   | Advanced users / customization  |

---

## 💻 Basic Terminal Commands

```bash
pwd          # Print Working Directory — where am I?
ls           # List files
ls -la       # List ALL files (including hidden) with details
cd /         # Go to root directory
cd ~         # Go to home directory
cd ..        # Go one level up
cd -         # Go back to previous directory
clear        # Clear the screen (or Ctrl + L)


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


✨ Notes
The kernel is the core of the OS — it talks directly to hardware.
The shell is how you interact with the system using commands.
Everything in Linux is treated as a file (including devices and processes

