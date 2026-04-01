# LESSON:3 - Users & Permissions 

# USERS & GROUPS
whoami           # Who am I logged in as?
id               # Shows your UID, GID, groups
who              # Who is logged into the system?
sudo su -        # Switch to root user
adduser alice    # Create new user
passwd alice     # Set password for alice
usermod -aG sudo alice   # Add alice to sudo group
groups alice     # Show alice's groups


# FILE PERMISSIONS
ls -la
# Output looks like:
# -rwxr-xr-- 1 alice devs 1234 Mar 13 10:00 script.sh
#  ↑↑↑↑↑↑↑↑↑
#  │└──┬──┘└──┬──┘└──┬──┘
#  │  owner  group  others
#  └─ file type (- = file, d = dir, l = symlink)
```
```
r = read    = 4
w = write   = 2
x = execute = 1


chmod 755 script.sh   # rwxr-xr-x  (owner=7, group=5, others=5)
chmod 644 file.txt    # rw-r--r--  (common for config files)
chmod +x script.sh    # Just add execute permission
chown alice:devs file.txt   # Change owner AND group


💼 Interview classic: "What does chmod 777 do and why is it dangerous?"
Answer: Gives everyone full read/write/execute — dangerous because any user or process can modify or execute that file

