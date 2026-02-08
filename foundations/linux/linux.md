---
title: "Linux"
description: "Get hands-on with Linux — terminal commands, package management, file operations, and user management on Ubuntu."
sidebar_position: 1
---

# Linux

Linux powers the vast majority of servers, cloud instances, and containers in production today. Working comfortably in a Linux terminal is a non-negotiable skill for any infrastructure or DevOps role.

## What Is Linux?

Linux is a free, open-source operating system kernel created by Linus Torvalds in 1991. On its own, a kernel just manages hardware and processes. Combined with a distribution (or "distro") like Ubuntu, Debian, or Red Hat Enterprise Linux, it becomes a complete operating system with package managers, utilities, and configuration tools. Most cloud workloads, containers, and CI/CD runners use Linux because it is stable, scriptable, and free to deploy at scale. When you spin up an EC2 instance on AWS or a VM on Azure, you are almost certainly booting a Linux distribution.

Throughout this section, we will use Ubuntu as our reference distribution. The commands and concepts transfer directly to other Debian-based distros and, with minor differences in package management, to RHEL-family systems as well.

## Why It Matters

If you are going to work with cloud platforms, containers, CI/CD pipelines, or infrastructure as code, you will be working in Linux. Being fluent with the terminal, file system, and package management is essential for every section that follows. When a deployment fails at 2 a.m. and you need to SSH into a production server to read logs, there is no graphical interface to help you. The terminal is your only tool, and comfort with it separates capable engineers from those who freeze under pressure.

## What You'll Learn

- Navigating the terminal and running commands
- Package management with `apt`
- File and directory operations (`ls`, `cp`, `mv`, `rm`, `mkdir`)
- File permissions with `chmod` and `chown`
- User and group management
- Environment variables and the `PATH`
- Reading logs and system information

---

## Getting a Linux Environment

Before you can practice any of the commands in this section, you need access to a Linux terminal. Here are the most common options, from easiest to most involved.

| Option | Best For | Cost |
|---|---|---|
| WSL (Windows Subsystem for Linux) | Windows users who want Linux alongside Windows | Free |
| Cloud VM (AWS, Azure, GCP free tier) | Anyone who wants a real remote server to practice on | Free tier available |
| Docker container | Users who already have Docker installed | Free |
| Native Linux install or dual boot | Users ready to commit to Linux as a primary or secondary OS | Free |

### WSL on Windows

If you are on Windows 10 or 11, WSL gives you a full Ubuntu terminal without leaving Windows. Open PowerShell as Administrator and run:

```powershell
wsl --install
```

This installs Ubuntu by default. After a reboot, open the "Ubuntu" app from the Start menu. You will be prompted to create a username and password. Once you see the `$` prompt, you are in a real Linux environment.

### Cloud VM

Launching a small VM on a cloud provider is an excellent way to practice because it mirrors real-world workflows. On AWS, launch a `t2.micro` or `t3.micro` instance with the Ubuntu Server AMI. On Azure, create a B1s VM with Ubuntu. Both fall within free-tier limits for new accounts. Once the VM is running, connect with SSH:

```bash
ssh -i ~/path/to/key.pem ubuntu@<public-ip>
```

You will use SSH extensively in later sections covering [Networking](/learn/foundations/networking-fundamentals/) and cloud platforms, so getting comfortable with it now pays dividends.

### Docker Container

If Docker is already on your machine, you can spin up a disposable Ubuntu environment in seconds:

```bash
docker run -it ubuntu:24.04 bash
```

This drops you into a root shell inside an Ubuntu container. Anything you install or change disappears when you exit unless you commit the container. That disposability makes it perfect for experimentation.

> **Try It**: Pick one of the options above and get to a Linux prompt. Run `cat /etc/os-release` to confirm you are on Ubuntu. You should see output that includes `NAME="Ubuntu"` and a version number.

---

## The Terminal and Shell

The **terminal** (or terminal emulator) is the window that displays text. The **shell** is the program running inside that window, interpreting your commands and returning output. On Ubuntu, the default shell is **Bash** (Bourne Again SHell). Other shells exist (Zsh, Fish, Dash), but Bash is the standard you will encounter on nearly every server.

### The Prompt

When Bash is ready for input, it shows a prompt. A typical Ubuntu prompt looks like this:

```
cloudchase@ubuntu-server:~/projects$
```

| Segment | Meaning |
|---|---|
| `cloudchase` | Your username |
| `@ubuntu-server` | The hostname of the machine |
| `~/projects` | Your current working directory (`~` means your home directory) |
| `$` | You are a regular user (`#` means you are root) |

### Command Structure

Every command follows the same general pattern:

```
command [options] [arguments]
```

- **command**: The program to run (`ls`, `cp`, `mkdir`).
- **options** (also called flags): Modify behavior. Short flags use a single dash (`-l`), long flags use double dashes (`--all`). Short flags can often be combined (`-la` instead of `-l -a`).
- **arguments**: The targets the command acts on (file names, directory paths, usernames).

Example:

```bash
ls -lh /var/log
```

Here, `ls` is the command, `-lh` combines two flags (`-l` for long format, `-h` for human-readable sizes), and `/var/log` is the argument (the directory to list).

### Getting Help

You do not need to memorize every flag. Linux gives you two built-in reference systems:

```bash
man ls          # opens the full manual page for ls
ls --help       # prints a shorter usage summary
```

Inside a `man` page, press `q` to quit, `/` to search, and `n` to jump to the next match. You will learn more about navigating text in [Text Editing](/learn/foundations/text-editing/).

### Tab Completion

Pressing **Tab** after typing part of a command, filename, or path will auto-complete it. If there are multiple matches, pressing Tab twice shows all possibilities. This saves time and prevents typos. Get in the habit of using it constantly.

---

## File System Navigation

Linux organizes everything in a single directory tree rooted at `/`. There are no drive letters like Windows. Every file, device, and process has a path that starts from `/`.

### Key Directories

| Directory | Purpose |
|---|---|
| `/` | Root of the entire file system |
| `/home` | User home directories (`/home/cloudchase`) |
| `/etc` | System configuration files |
| `/var` | Variable data: logs, caches, mail |
| `/tmp` | Temporary files (cleared on reboot) |
| `/usr` | User programs and libraries |
| `/bin`, `/sbin` | Essential system binaries |
| `/opt` | Optional/third-party software |
| `/dev` | Device files |
| `/proc` | Virtual file system exposing kernel and process info |

### Navigation Commands

**`pwd`** prints your current working directory:

```bash
pwd
```

```
/home/cloudchase
```

**`ls`** lists directory contents. The flags you will use most often:

```bash
ls            # basic listing
ls -l         # long format (permissions, owner, size, date)
ls -a         # show hidden files (names starting with .)
ls -lh        # long format with human-readable file sizes
ls -la        # combine long format and hidden files
```

Example output of `ls -lh`:

```
total 12K
drwxr-xr-x 2 cloudchase cloudchase 4.0K Jan 15 09:30 documents
-rw-r--r-- 1 cloudchase cloudchase  412 Jan 14 16:22 notes.txt
-rwxr-xr-x 1 cloudchase cloudchase  89  Jan 14 16:25 setup.sh
```

Each column tells you something: permissions, link count, owner, group, size, modification date, and name. We will break down the permissions column later in this section.

**`cd`** changes your working directory:

```bash
cd /var/log          # absolute path — starts from /
cd documents         # relative path — moves into documents/ under the current directory
cd ..                # move up one level
cd ../..             # move up two levels
cd ~                 # jump to your home directory (same as just cd)
cd -                 # jump to the previous directory you were in
```

The difference between absolute and relative paths is important. An **absolute path** starts with `/` and specifies the full location from root (`/home/cloudchase/projects`). A **relative path** starts from your current directory (`projects/app` means "from where I am now, go into projects, then app").

> **Try It**: Navigate around the file system. Start at your home directory with `cd ~`, then move to `/etc`, list its contents with `ls`, go back home with `cd -`, and confirm your location with `pwd`. Try using Tab completion while typing paths.

---

## File Operations

These are the commands you will use dozens of times a day to create, move, copy, read, and delete files.

### Creating Files and Directories

**`touch`** creates an empty file (or updates the timestamp of an existing one):

```bash
touch notes.txt
touch file1.txt file2.txt file3.txt   # create multiple files at once
```

**`mkdir`** creates a directory. The `-p` flag creates parent directories as needed:

```bash
mkdir projects
mkdir -p projects/webapp/src          # creates projects/, webapp/, and src/ in one shot
```

### Copying, Moving, and Renaming

**`cp`** copies files. Use `-r` (recursive) to copy directories:

```bash
cp notes.txt notes-backup.txt         # copy a file
cp -r projects/ projects-backup/      # copy an entire directory tree
```

**`mv`** moves or renames files and directories:

```bash
mv notes.txt documents/               # move notes.txt into documents/
mv old-name.txt new-name.txt          # rename a file
mv projects/ ~/archived-projects/     # move a directory
```

### Deleting

**`rm`** removes files. Use `-r` for directories. The `-f` flag forces deletion without prompting:

```bash
rm notes.txt                          # delete a single file
rm -r projects-backup/               # delete a directory and its contents
rm -rf temp/                          # force-delete without confirmation
```

There is no recycle bin in the terminal. `rm` is permanent. Double-check your command before pressing Enter, especially with `-rf` and wildcards.

### Reading File Contents

**`cat`** prints the entire contents of a file:

```bash
cat /etc/hostname
```

```
ubuntu-server
```

**`less`** opens a file in a scrollable viewer (press `q` to quit, `/` to search):

```bash
less /var/log/syslog
```

**`head`** and **`tail`** show the first or last lines of a file. The `-n` flag controls how many lines:

```bash
head -n 5 /var/log/syslog             # first 5 lines
tail -n 20 /var/log/syslog            # last 20 lines
tail -f /var/log/syslog               # follow — watch new lines appear in real time
```

The `tail -f` command is invaluable for watching logs during deployments. You will use it constantly.

### Finding Files

**`find`** searches for files by name, type, modification time, and more:

```bash
find /etc -name "*.conf"                     # find all .conf files under /etc
find /home -type d -name "projects"          # find directories named "projects"
find /var/log -mtime -1                      # files modified in the last 24 hours
find . -name "*.tmp" -delete                 # find and delete all .tmp files
```

The `.` in the last example means "start searching from the current directory."

### Counting

**`wc`** (word count) counts lines, words, and characters:

```bash
wc -l /etc/passwd                     # count lines (each line is one user account)
```

```
35 /etc/passwd
```

> **Try It**: Create a project structure: `mkdir -p ~/practice/src ~/practice/docs`. Create files with `touch ~/practice/src/app.py ~/practice/docs/readme.txt`. Copy the entire `practice` directory to `practice-backup`. List both directories with `ls -R ~/practice ~/practice-backup` to confirm. Then clean up with `rm -r ~/practice-backup`.

---

## Package Management

A **package manager** automates the process of installing, updating, and removing software. Instead of downloading executables from websites, you tell the package manager what you want and it handles downloading, dependency resolution, and installation. On Ubuntu and other Debian-based distributions, the package manager is **APT** (Advanced Package Tool).

### Updating the Package Index

Before installing anything, update the local list of available packages so APT knows what is current:

```bash
sudo apt update
```

This does not upgrade any software. It just refreshes the catalog. You should run this before every install session.

### Installing Packages

```bash
sudo apt install tree                 # install the tree utility
sudo apt install curl wget git        # install multiple packages at once
```

APT pulls the package and any dependencies from **repositories** — servers that host verified software packages. Ubuntu's default repositories cover thousands of packages. You can add third-party repositories when you need software that is not in the defaults.

### Removing Packages

```bash
sudo apt remove tree                  # remove the package but keep config files
sudo apt purge tree                   # remove the package and its config files
sudo apt autoremove                   # clean up unused dependency packages
```

### Upgrading Installed Packages

```bash
sudo apt upgrade                      # upgrade all installed packages to latest versions
```

Run `sudo apt update` first so APT knows about the latest versions.

### Searching and Listing

```bash
apt search nginx                      # search for packages related to nginx
apt show nginx                        # show detailed info about a package
apt list --installed                  # list all installed packages
apt list --installed | grep python    # filter for packages with "python" in the name
```

The `|` (pipe) in the last command sends the output of `apt list` into `grep`, which filters for matching lines. Piping is a fundamental concept you will explore more in [Shell Scripting](/learn/foundations/shell-scripting/).

> **Try It**: Update your package index, then install `tree` (a utility that displays directory structures as a tree). Run `tree ~/practice` to see the directory structure you created earlier. Then remove `tree` with `sudo apt remove tree`.

---

## Permissions and Ownership

Every file and directory in Linux has three attributes that control access: an **owner**, a **group**, and a set of **permissions**. You encountered these concepts in [OS Fundamentals](/learn/foundations/os-fundamentals/). Now you will learn to read and change them.

### Reading Permissions

Run `ls -l` and look at the first column:

```
-rwxr-xr-- 1 cloudchase developers 2048 Jan 15 10:00 deploy.sh
drwxr-x--- 2 cloudchase developers 4096 Jan 15 10:00 scripts
```

The permission string has 10 characters:

| Position | Meaning |
|---|---|
| 1 | File type: `-` = regular file, `d` = directory, `l` = symbolic link |
| 2-4 | Owner permissions (user) |
| 5-7 | Group permissions |
| 8-10 | Other (everyone else) permissions |

Each set of three characters uses `r` (read), `w` (write), and `x` (execute). A `-` means that permission is not granted.

For the file `deploy.sh` above (`-rwxr-xr--`):

- **Owner** (`cloudchase`): read, write, execute
- **Group** (`developers`): read, execute
- **Other**: read only

For directories, `x` means the ability to enter the directory (use `cd` into it), and `r` means the ability to list its contents.

### Changing Permissions with chmod

`chmod` changes permissions. You can use **symbolic notation** or **numeric notation**.

#### Symbolic Notation

Symbolic notation uses letters to specify who, what action, and which permission:

- **Who**: `u` (user/owner), `g` (group), `o` (other), `a` (all)
- **Action**: `+` (add), `-` (remove), `=` (set exactly)
- **Permission**: `r`, `w`, `x`

```bash
chmod u+x script.sh                   # give the owner execute permission
chmod g-w config.yaml                 # remove write from group
chmod o=r notes.txt                   # set other to read-only (removes w and x if set)
chmod a+r readme.txt                  # give everyone read permission
chmod u+rwx,g+rx,o+r deploy.sh       # set multiple at once
```

#### Numeric Notation

Numeric notation represents each permission set as a single digit. Each permission has a value:

| Permission | Value |
|---|---|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| None | 0 |

Add the values together for each position (owner, group, other):

- `rwx` = 4 + 2 + 1 = **7**
- `r-x` = 4 + 0 + 1 = **5**
- `r--` = 4 + 0 + 0 = **4**
- `---` = 0 + 0 + 0 = **0**

```bash
chmod 755 deploy.sh                   # rwxr-xr-x
chmod 644 config.yaml                 # rw-r--r--
chmod 600 id_rsa                      # rw-------
chmod 700 ~/bin                       # rwx------
```

### Common Permission Patterns

Memorize these. You will use them constantly:

| Numeric | Symbolic | Use Case |
|---|---|---|
| `755` | `rwxr-xr-x` | Executable scripts, public directories |
| `644` | `rw-r--r--` | Configuration files, regular text files |
| `600` | `rw-------` | SSH private keys, secrets, credentials |
| `700` | `rwx------` | Private directories, home directories |
| `444` | `r--r--r--` | Read-only files, locked configs |

SSH is particularly strict about key file permissions. If your private key has permissions wider than `600`, SSH will refuse to use it and print a warning. You will see this firsthand in the [Networking](/learn/foundations/networking-fundamentals/) section.

### Changing Ownership with chown

`chown` changes the owner and/or group of a file:

```bash
sudo chown cloudchase deploy.sh              # change owner
sudo chown cloudchase:developers deploy.sh   # change owner and group
sudo chown :developers deploy.sh             # change group only
sudo chown -R cloudchase:developers project/ # recursive — apply to everything inside
```

Changing ownership almost always requires `sudo` because only root can give files to other users.

> **Try It**: Create a file with `touch ~/practice/secret.txt`. Check its default permissions with `ls -l ~/practice/secret.txt`. Change the permissions to `600` (owner read/write only) with `chmod 600 ~/practice/secret.txt`. Verify with `ls -l` again. Then try `chmod 755` and check once more.

---

## User and Group Management

Linux is a multi-user system. Even on a single-person VM, understanding user and group management matters because services (nginx, PostgreSQL, Docker) run as their own users for security isolation.

### Identifying Yourself

```bash
whoami                                # prints your username
```

```
cloudchase
```

```bash
id                                    # prints your user ID, group ID, and group memberships
```

```
uid=1000(cloudchase) gid=1000(cloudchase) groups=1000(cloudchase),27(sudo),999(docker)
```

### Creating and Modifying Users

```bash
sudo useradd -m -s /bin/bash newuser  # create user with home directory and bash shell
sudo passwd newuser                   # set a password for the new user
```

The `-m` flag ensures a home directory is created at `/home/newuser`. The `-s` flag sets the default shell. Without `-s`, the user might get `/bin/sh` or no shell at all.

To modify an existing user:

```bash
sudo usermod -aG docker cloudchase    # add cloudchase to the docker group
sudo usermod -aG sudo newuser         # give newuser sudo access
```

The `-aG` flags mean "append to group." Without `-a`, the user would be removed from all other supplementary groups and placed only in the specified one. This is a common mistake that can lock users out of critical groups.

### sudo and Root Access

The **root** user (UID 0) has unrestricted access to the entire system. Logging in as root directly is dangerous because any mistake is catastrophic. Instead, Linux uses `sudo` ("superuser do") to let authorized users run individual commands with root privileges:

```bash
sudo apt update                       # run apt update as root
sudo cat /etc/shadow                  # read a file only root can access
```

The system checks `/etc/sudoers` to decide who is allowed to use `sudo`. On Ubuntu, any user in the `sudo` group can use it. Never edit `/etc/sudoers` directly. Use `visudo` instead, which validates the syntax before saving:

```bash
sudo visudo
```

### Switching Users

```bash
su - newuser                          # switch to newuser (prompts for their password)
sudo su -                             # switch to root (uses your sudo password)
exit                                  # return to your previous user
```

The `-` ensures you get a full login shell with the target user's environment variables and home directory.

### Groups

Groups let you grant permissions to multiple users at once. A user can belong to many groups.

```bash
cat /etc/group                        # list all groups on the system
groups cloudchase                     # show which groups a user belongs to
sudo groupadd developers             # create a new group
sudo usermod -aG developers cloudchase  # add a user to the group
```

A common workflow: create a `developers` group, set a project directory to be owned by that group, then add team members to the group so they all have access.

```bash
sudo groupadd developers
sudo chown -R :developers /opt/project
sudo chmod -R 775 /opt/project
sudo usermod -aG developers alice
sudo usermod -aG developers bob
```

Now both `alice` and `bob` can read, write, and execute files in `/opt/project`.

> **Try It**: Run `whoami`, then `id` to see your groups. Create a new user with `sudo useradd -m -s /bin/bash testuser`. Verify the user exists by running `id testuser`. Then clean up with `sudo userdel -r testuser` (the `-r` flag removes the home directory too).

---

## Environment Variables and PATH

Environment variables are key-value pairs that configure the behavior of the shell and the programs you run. They are how Linux passes configuration into processes without hardcoding values.

### Viewing Environment Variables

```bash
echo $HOME                            # prints your home directory
echo $USER                            # prints your username
echo $SHELL                           # prints your default shell
echo $PATH                            # prints the list of directories searched for commands
env                                   # prints all environment variables
printenv HOME                         # prints the value of a specific variable
```

### Setting Variables

```bash
MY_VAR="hello"                        # set a shell variable (only available in this session)
echo $MY_VAR
```

```
hello
```

A shell variable exists only in the current shell. To make it available to child processes (programs you launch from this shell), you need to **export** it:

```bash
export MY_VAR="hello"                 # now child processes can see MY_VAR
export API_URL="https://api.example.com"
```

### Understanding PATH

`PATH` is the most important environment variable. It is a colon-separated list of directories. When you type a command like `python3`, the shell searches each directory in `PATH` from left to right until it finds an executable with that name.

```bash
echo $PATH
```

```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

If you install a program to `/opt/myapp/bin`, the shell will not find it unless that directory is in your `PATH`:

```bash
export PATH="/opt/myapp/bin:$PATH"
```

This prepends `/opt/myapp/bin` to the front of `PATH`, so the shell searches it first.

You can verify which executable the shell will use for a given command:

```bash
which python3                         # prints the full path to the python3 binary
```

```
/usr/bin/python3
```

### Making Changes Permanent

Variables set with `export` disappear when you close the terminal. To make them permanent, add the `export` lines to one of your shell configuration files:

- **`~/.bashrc`**: Runs every time you open a new interactive Bash shell. Best for aliases, functions, and environment variables you always want.
- **`~/.profile`** (or `~/.bash_profile`): Runs once at login. Best for `PATH` modifications and variables that only need to be set once per session.

```bash
echo 'export PATH="/opt/myapp/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc                      # reload the file without opening a new terminal
```

The `source` command (or its shorthand `.`) re-reads the file in your current shell. You will work with these configuration files extensively in [Shell Scripting](/learn/foundations/shell-scripting/).

> **Try It**: Run `echo $PATH` to see your current path. Create a directory `~/bin` with `mkdir -p ~/bin`. Add it to your PATH with `export PATH="$HOME/bin:$PATH"`. Verify with `echo $PATH`. Create a simple script: write `echo "it works"` into `~/bin/hello.sh`, make it executable with `chmod +x ~/bin/hello.sh`, and run it by typing just `hello.sh`.

---

## Logs and System Information

When something goes wrong on a Linux system, and something always eventually goes wrong, logs are where you find answers. Understanding how to read system information and navigate log files is essential for troubleshooting.

### System Information

These commands give you a quick overview of the machine you are on:

```bash
uname -a                              # kernel version, architecture, hostname
```

```
Linux ubuntu-server 6.8.0-45-generic #45-Ubuntu SMP x86_64 GNU/Linux
```

```bash
hostname                              # just the hostname
uptime                                # how long the system has been running
```

```
 10:32:15 up 14 days,  3:21,  2 users,  load average: 0.12, 0.08, 0.05
```

The three **load average** numbers show system load over the last 1, 5, and 15 minutes. On a single-core machine, a load average of 1.0 means the CPU is fully utilized. On a 4-core machine, 4.0 means full utilization.

### Disk Usage

```bash
df -h                                 # disk free — show filesystem usage
```

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   12G   36G  25% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
```

```bash
du -sh /var/log                       # disk usage — size of a specific directory
du -sh /var/log/*                     # size of each item inside /var/log
```

### Memory Usage

```bash
free -h                               # show memory (RAM) and swap usage
```

```
              total        used        free      shared  buff/cache   available
Mem:          3.8Gi       1.2Gi       1.4Gi        12Mi       1.2Gi       2.4Gi
Swap:         2.0Gi          0B       2.0Gi
```

The **available** column is more useful than **free** because Linux uses spare RAM for disk caching. The available number reflects how much memory can actually be used by new processes.

### Log Files

Most system logs live in `/var/log/`:

| File | Contains |
|---|---|
| `/var/log/syslog` | General system messages |
| `/var/log/auth.log` | Authentication events (logins, sudo usage, SSH) |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/apt/history.log` | Package install/remove history |
| `/var/log/cloud-init.log` | Cloud instance initialization (on cloud VMs) |

You can read these with `cat`, `less`, `tail`, or `grep`:

```bash
sudo tail -n 50 /var/log/syslog       # last 50 lines of the system log
sudo tail -f /var/log/auth.log        # watch authentication events in real time
sudo grep "error" /var/log/syslog     # search for lines containing "error"
sudo grep -i "fail" /var/log/auth.log # case-insensitive search for "fail"
```

### journalctl

On systems using **systemd** (which includes Ubuntu 16.04 and later), `journalctl` provides a unified interface to all system logs:

```bash
sudo journalctl                       # all logs (very long — use less-style navigation)
sudo journalctl -u nginx              # logs for a specific service (e.g., nginx)
sudo journalctl -u ssh -f             # follow SSH service logs in real time
sudo journalctl --since "1 hour ago"  # logs from the last hour
sudo journalctl --since "2025-01-15 09:00" --until "2025-01-15 10:00"  # time range
sudo journalctl -p err                # only error-level messages and above
```

### dmesg

`dmesg` shows kernel ring buffer messages, which are useful for diagnosing hardware and driver issues:

```bash
sudo dmesg | tail -20                 # last 20 kernel messages
sudo dmesg -T                         # show human-readable timestamps
```

You will see messages about disk drives being detected, network interfaces coming up, and any hardware errors.

> **Try It**: Run `uname -a` to see your kernel version, `df -h` to check disk space, and `free -h` to check memory. Then look at recent authentication events with `sudo tail -n 20 /var/log/auth.log`. If you are on a systemd system, try `sudo journalctl --since "10 minutes ago"` to see what has been logged recently.

---

## Key Takeaways

- Linux is the dominant operating system for servers, cloud, and containers. Terminal fluency is essential for infrastructure work.
- The shell interprets commands in the format `command [options] [arguments]`. Use `man` and `--help` when you need reference.
- Navigation revolves around `pwd`, `ls`, and `cd`. Understand the difference between absolute (`/home/user`) and relative (`./docs`) paths.
- File operations (`touch`, `mkdir`, `cp`, `mv`, `rm`, `cat`, `less`, `head`, `tail`, `find`) are your daily toolkit. Remember that `rm` is permanent.
- APT is Ubuntu's package manager. Always run `sudo apt update` before installing packages.
- Permissions follow the `rwx` model for owner, group, and other. Know the numeric equivalents: `755` for scripts, `644` for configs, `600` for secrets.
- `chown` changes file ownership. `chmod` changes permissions. Both are critical for security.
- User and group management lets you control access across a multi-user system. Use `sudo` instead of logging in as root.
- Environment variables configure shell and program behavior. `PATH` determines where the shell looks for executables. Persist changes in `~/.bashrc`.
- Logs live in `/var/log/` and are accessible through `journalctl`. When something breaks, logs are the first place to look.
