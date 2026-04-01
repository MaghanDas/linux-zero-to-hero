
# Exercise 1 - Navigate the system
``` bash
pwd
ls -la /
ls -la /etc | head -20
cd /var/log && ls

# Exercise 2 - Create your practice workspace
mkdir -p ~/linux-practice/lesson1
cd ~/linux-practice/lesson1
touch notes.txt
echo "Linux is awesome" > notes.txt
cat notes.txt

# Exercise 3 - Permissions
ls -la ~/linux-practice/
chmod 700 ~/linux-practice/lesson1
ls -la ~/

# Exercise 4 - Explore
cat /etc/os-release        # What distro am I on?
uname -a                   # Kernel version info
uptime                     # How long has system been running?
df -h                      # Disk usage
free -h                    # RAM usage

🎯 Key Takeaways from Lesson 1-3
ConceptRemember ThisEverything is a fileEven devices live in /dev//etcAll config fileschmod 755Standard for executablestail -fReal-time log monitoringsudoRun as root temporarily

