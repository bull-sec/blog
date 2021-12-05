# Linux Privilege Escalation

We essentially have three areas to focus on when we're assessing a Linux machine for privilege escalation vulnerabilities, these can be further broken down, but the main three are Exploits, Credential Access, and Misconfiguration

- Exploits
  - Kernel Version
  - Binary File Version
  - Service Running on localhost

---

- Credential Access
  - Reused Passwords (Password Spraying)
    - Credentials from Configuration Files
    - Credentials from local databases
    - Credentials from Bash History
    - SSH Keys
    - Sudo Access
    - Group Privileges (Docker, LXD, etc)

---

- Misconfiguration
    - Cron Jobs
        - Writeable Cron Job
        - Writeable Cron Job Dependency File (Python Library, etc)
    - SUID/SGID Files
    - Interesting Binary Capabilities
    - Sensitive Files Writeable
        - `/etc/passwd`
        - `/etc/shadow`
        - `/etc/sudoers`
        - Configuration Files
    - Sensitive Files Readable
        - `/etc/shadow`
        - `/path/.ssh/id_rsa` (SSH Keys)
    - Writeable Path
        - `root` $PATH Writeable
        - Directory in $PATH Writeable
    - `LD_PRELOAD` set in `/etc/sudoers`
---

## Exploits

```bash
# Kernel Version

uname -a
uname -r #just kernel version
cat /etc/*release
./linux-exploit-suggester

# Services Running on localhost (127.0.0.1)

netstat -planetu

# Binary File Version

/path/to/file --help
/path/to/file --version

# Get additional information about a Binary
strace /path/to/file # system calls
ltrace /path/to/file # library calls
```

## Credential Access

```bash
"Remember to check whatever application was exploited to gain access to find additional credentials for databases and other services."

# Reused Passwords (Password Spraying)

cat /etc/passwd | grep -v nologin # get a list of real users to check for password spraying potential

# Credentials from Configuration Files

grep pass /etc/* # grep for passwords

grep -Hirl pass /etc/* # dump filenames of files containing passwords

# Credentials from local databases
mysql -u <username> -p<password> -d <database

# Credentials from Bash History
cat /home/*/.bash_history # usually zeroed, but worth a check
cat /home/*/.zsh_history # Not everyone uses BASH

# SSH Keys
ls -la /home/*/.ssh/
find / -name "id_rsa*" 2>/dev/null

# Sudo Access
sudo -l

# Group Privileges (Docker, LXD, etc)

groups
cat /etc/groups
```

## Misconfiguration

```bash
#Cron Jobs

drwxr-xr-x   2 root     root      4096 Aug 22 14:18 cron.d
drwxr-xr-x   2 root     root      4096 Aug 20 22:51 cron.daily
drwxr-xr-x   2 root     root      4096 Jul 26 16:58 cron.hourly
drwxr-xr-x   2 root     root      4096 Jul 26 16:58 cron.monthly
-rw-r--r--   1 root     root      1042 Feb 10  2020 crontab
drwxr-xr-x   2 root     root      4096 Jul 26 16:58 cron.weekly

(Check scheduled tasks)

## Writeable Cron Job

blah

## Writeable Cron Job Dependency File (Python Library, etc)

blah

# SUID/SGID Files

find / -perm -u=s -type f 2>/dev/null
find / -perm -u=g -type f 2>/dev/null

# Interesting Binary Capabilities

getcap -r /bin/* /usr/bin/* /* 2>/dev/null

# Sensitive Files Writeable/Readable

ls -la /etc/passwd
ls -la /etc/shadow
ls -la /etc/sudoers
find / -name "id_rsa" -type f 2>/dev/null

# Configuration Files

find / -name "*.conf" -writable
find /etc/ -name "*.conf" -exec ls -la {} \; 2>/dev/null

# Writeable Path

"Remember that relative and absolute paths are important"

export PATH=.:$PATH # current directory at start of $PATH
export PATH=$PATH:/some/path # some directory at end of $PATH

"Can you write to the global $PATH?"

sudo -l
cat /etc/sudoers # probably don't have access to read this
"env_reset" # means you can't abuse $PATH

# Directory in $PATH Writeable
for x in $(echo $PATH | sed 's/:/\n/g');do ls -lahd $x ; done
```

