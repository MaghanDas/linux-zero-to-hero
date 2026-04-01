
# LESSON:2 - THE TERMINAL

---

## BASIC NAVIGATION

```bash
pwd          # Print Working Directory — where am I right now?
ls           # List files
ls -la       # List ALL files including hidden, with details
cd /         # Go to root directory
cd ~         # Go to your home directory
cd ..        # Go one level up
cd -         # Go back to previous directory
clear        # Clear the screen (or Ctrl+L)

## Understanding the File System — Critical!
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



# Everything in linux is a file - even hardware devices, network sockets, and processes. this is a core linux philosphy.


# Working with files.
touch file.txt           # Create empty file
mkdir myfolder           # Create directory
mkdir -p a/b/c           # Create nested dirs at once
cp file.txt backup.txt   # Copy file
mv file.txt /tmp/        # Move file
rm file.txt              # Delete file
rm -rf myfolder/         # Delete folder recursively (be careful!)
cat file.txt             # Print file contents
less file.txt            # View file page by page (q to quit)
head -n 5 file.txt       # First 5 lines
tail -n 5 file.txt       # Last 5 lines
tail -f /var/log/syslog  # LIVE follow a log file


