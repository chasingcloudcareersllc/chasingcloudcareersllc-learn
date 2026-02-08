# Foundations Path — Syllabus

## Preamble

### Purpose

This document is the **authoritative source of truth** for what the Foundations learning path covers. Every topic, subtopic, learning objective, and exercise type defined here is what the content must deliver. If it is not in this syllabus, it is not in scope. If it is in this syllabus, it must be in the content.

### How to use this document

- **Content authors**: use the per-section outlines to build lessons. Every bullet under "Topic Outline" becomes content. Every learning objective becomes something the learner demonstrably achieves.
- **Reviewers**: verify published content against this syllabus for completeness and depth.
- **Learners**: use the learning objectives and proficiency criteria to self-assess.

### What "deep expert" means

A learner who completes this path can:

- **Explain internals** — not just use a tool, but describe how it works underneath.
- **Troubleshoot edge cases** — diagnose problems others cannot using first-principles knowledge.
- **Make architectural decisions** — choose between approaches with informed tradeoffs.
- **Teach others** — explain concepts clearly enough to bring someone else up to speed.

This goes beyond day-one job requirements. The goal is foundational mastery that compounds over an entire career.

### Content conventions

- **Zero assumptions**: the learner has never touched a computer professionally.
- **Provider-agnostic**: teach concepts first, tools second. When referencing cloud services, mention AWS, Azure, and GCP equivalents. When referencing distros, cover apt/dnf/pacman differences. When referencing CI/CD, use GitHub Actions as the primary example but mention GitLab CI, Jenkins, and CircleCI.
- **Links**: only link to official documentation for named technologies.
- **Tone**: punchy and succinct. No filler. Every sentence earns its place.

---

## Path Overview

### Sections

| # | Title | Icon | One-line description |
|---|-------|------|----------------------|
| 1 | Getting Started | compass | Career landscape, learning strategy, and how to get the most out of this path. |
| 2 | Introduction to Computers | monitor | Hardware, software, binary, memory hierarchy, and the boot process — from silicon to screen. |
| 3 | OS Fundamentals | cpu | Processes, memory management, file systems, permissions, and the kernel — what the OS actually does. |
| 4 | Linux | terminal | Terminal fluency — commands, filesystem hierarchy, package management, user administration, and system internals. |
| 5 | Text Editing | pen-tool | Vim mastery — modes, motions, registers, macros, configuration, and the Unix text-processing philosophy. |
| 6 | Shell Scripting | file-code | Bash automation — variables, control flow, pipes, process substitution, error handling, and cron. |
| 7 | Programming | code | Python fundamentals — syntax, data structures, functions, OOP, file I/O, virtual environments, and packages. |
| 8 | Version Control | git-branch | Git and collaborative workflows — commits, branches, merges, rebases, pull requests, and repository management. |
| 9 | Networking Fundamentals | network | IP, TCP/UDP, DNS, HTTP, routing, firewalls, SSH, and packet-level analysis. |
| 10 | API Fundamentals | plug | REST, HTTP methods, authentication, JSON/YAML, curl, Python requests, and API design principles. |
| 11 | Security Fundamentals | shield | CIA triad, encryption, PKI, authentication, secrets management, hardening, and common attack vectors. |
| 12 | CI/CD | refresh-cw | Automated pipelines — workflows, triggers, jobs, artifacts, secrets, testing, and deployment strategies. |
| 13 | Containers | container | Container internals — images, runtimes, Dockerfiles, volumes, networking, Compose, and OCI spec. |
| 14 | Container Orchestration | ship | Kubernetes — pods, deployments, services, networking, storage, RBAC, operators, and cluster internals. |
| 15 | Infrastructure as Code | blocks | Terraform — providers, resources, state, modules, testing, policy, and multi-environment workflows. |

### Dependency Graph

```
1 Getting Started
└─▶ 2 Introduction to Computers
    └─▶ 3 OS Fundamentals
        └─▶ 4 Linux
            ├─▶ 5 Text Editing
            │   └─▶ 6 Shell Scripting
            │       └─▶ 7 Programming
            │           ├─▶ 8 Version Control
            │           │   └─▶ 12 CI/CD
            │           └─▶ 10 API Fundamentals
            └─▶ 9 Networking Fundamentals
                ├─▶ 10 API Fundamentals
                └─▶ 11 Security Fundamentals
                    └─▶ 12 CI/CD
                        └─▶ 13 Containers
                            └─▶ 14 Container Orchestration
                                └─▶ 15 Infrastructure as Code
```

### Five Phases

1. **Understanding Machines** — Sections 1–3
2. **Working with Linux** — Sections 4–6
3. **Building & Collaborating** — Sections 7–9
4. **APIs & Security** — Sections 10–11
5. **Automation & Infrastructure** — Sections 12–15

---

## Section 1: Getting Started

- **Position**: 1
- **Icon**: compass
- **Prerequisites**: None

### Learning Objectives

1. You can describe what cloud computing, DevOps, SRE, and platform engineering roles do day-to-day.
2. You can explain the shared responsibilities across infrastructure, development, and operations roles.
3. You can map this path's 15 sections to the skills employers list in job postings.
4. You can set up a local learning environment (terminal emulator, virtual machine or cloud instance, text editor).
5. You can identify the difference between a certification, a degree, and demonstrable skill — and when each matters.
6. You can describe the T-shaped skill model and explain why breadth and depth both matter.
7. You can navigate official documentation for any technology and extract what you need.
8. You can articulate your own learning plan with concrete weekly milestones.

### Topic Outline

1. **The tech career landscape**
   - What cloud computing is and why it exists
   - Role taxonomy: DevOps engineer, SRE, cloud engineer, platform engineer, infrastructure engineer
   - What these roles share vs. where they diverge
   - Day-in-the-life breakdowns for each role
   - The job market: what employers actually screen for
2. **How this path works**
   - The 15-section structure and why it is sequenced this way
   - Phase groupings and how sections build on each other
   - Time investment expectations (not calendar estimates — effort benchmarks)
   - How to use exercises, proficiency criteria, and self-assessment
3. **Setting up your environment**
   - Choosing a terminal emulator (platform-specific: Terminal.app, Windows Terminal, GNOME Terminal, Alacritty, iTerm2)
   - Installing a hypervisor (VirtualBox, UTM, QEMU/KVM, Hyper-V) or provisioning a cloud instance
   - Choosing a Linux distribution for learning (Ubuntu, Fedora, Arch — why each, no single "right" answer)
   - Verifying your setup: SSH into your environment, run basic commands
4. **Learning strategy**
   - Active recall vs. passive review
   - Spaced repetition for command and concept retention
   - The value of breaking things intentionally
   - Building a personal lab notebook (plain text, Markdown, whatever sticks)
   - How to read documentation: man pages, --help, official docs, RFCs
5. **Certifications and credentials**
   - When certifications help (career switchers, HR filters, structured learning)
   - When they do not (replacing hands-on skill, credential-stacking without depth)
   - Major certification paths: AWS/Azure/GCP, CKA/CKAD, Terraform Associate, CompTIA, RHCSA
   - How to use this path as preparation for multiple certification tracks

### Key Concepts

- T-shaped skills: deep in one area, broad across many
- The build-break-fix learning loop
- Documentation as the primary information source

### Essential Commands and Tools

- `ssh` — connect to a remote machine
- `man` — read manual pages
- `--help` — inline help for any command
- Terminal emulator of choice
- Hypervisor or cloud provider console

### Exercises

- Set up a Linux VM or cloud instance from scratch and SSH into it
- Find and read the official documentation for three tools you have never used
- Write a one-page learning plan with weekly goals for the next 8 weeks
- Research two job postings and map their requirements to this path's sections

### Proficiency Criteria

You can explain the career landscape without buzzwords, you have a working Linux environment you built yourself, and you have a written plan for how you will work through this path. You treat official docs as your first stop, not a last resort.

---

## Section 2: Introduction to Computers

- **Position**: 2
- **Icon**: monitor
- **Prerequisites**: Section 1

### Learning Objectives

1. You can explain the function of every major hardware component (CPU, RAM, storage, GPU, NIC, motherboard, PSU) and how they interact.
2. You can describe the CPU instruction cycle (fetch-decode-execute) and explain pipelining, branch prediction, and superscalar execution.
3. You can convert between binary, decimal, and hexadecimal and explain why computers use binary.
4. You can explain the memory hierarchy (registers → L1/L2/L3 cache → RAM → storage) and why each layer exists.
5. You can describe the boot process from power-on through user login (POST → UEFI/BIOS → bootloader → kernel → init/systemd).
6. You can explain the difference between UEFI and BIOS, MBR and GPT.
7. You can describe what virtualization extensions (VT-x/AMD-V) do at the CPU level and why they matter.
8. You can explain the difference between system software and application software with concrete examples.
9. You can describe NUMA architecture and explain when it matters.
10. You can read a system specification sheet and understand every line item.

### Topic Outline

1. **Hardware components**
   - CPU: cores, threads, clock speed, instruction sets (x86_64, ARM), microarchitecture
   - CPU internals: ALU, control unit, registers, instruction pipeline, branch prediction, out-of-order execution, speculative execution
   - Virtualization extensions: VT-x, AMD-V, nested virtualization, hardware-assisted vs. software emulation
   - RAM: DRAM vs. SRAM, DDR generations, channels, ECC memory, registered vs. unbuffered
   - Memory hierarchy: registers (sub-nanosecond) → L1 cache (1ns) → L2 (3-5ns) → L3 (10-15ns) → RAM (50-100ns) → SSD (10-100μs) → HDD (1-10ms)
   - Cache coherence: MESI protocol, cache lines, false sharing
   - NUMA: non-uniform memory access, why multi-socket systems use it, performance implications
   - Storage: HDD (platters, seek time, rotational latency) vs. SSD (NAND flash, wear leveling, TRIM, NVMe vs. SATA)
   - GPU: parallel processing, CUDA/OpenCL basics, why GPUs are used for ML and rendering
   - NIC: Ethernet, speeds (1G/10G/25G/100G), hardware offloading (TCP offload, SR-IOV)
   - Motherboard: chipset, buses (PCIe lanes, bandwidth), form factors (ATX, mATX, ITX)
   - PSU: wattage, efficiency ratings (80 Plus), rails
   - Peripherals: USB standards, Thunderbolt, display interfaces (HDMI, DP)
2. **Number systems and data representation**
   - Binary: bits, bytes, words — why base-2
   - Hexadecimal: compact binary representation, common uses (memory addresses, colors, MAC addresses)
   - Integer representation: unsigned, signed (two's complement), overflow
   - Floating-point: IEEE 754 basics, why `0.1 + 0.2 != 0.3`
   - Character encoding: ASCII, Unicode, UTF-8, UTF-16 — why encoding matters
   - Endianness: big-endian vs. little-endian, network byte order
   - Units: bits vs. bytes, KB vs. KiB (SI vs. binary prefixes)
3. **Software layers**
   - Firmware: BIOS, UEFI, embedded controller firmware, firmware updates
   - System software: operating system, device drivers, system utilities
   - Application software: userland programs, interpreted vs. compiled
   - The kernel: what it is, what it does, kernel space vs. user space, system calls as the boundary
4. **The boot process**
   - Power-on: PSU power-good signal, CPU reset vector
   - POST: power-on self-test, beep codes, diagnostic LEDs
   - BIOS vs. UEFI: legacy vs. modern, Secure Boot, TPM
   - Partition tables: MBR (4 primary partitions, 2TB limit) vs. GPT (128 partitions, 9.4ZB)
   - Bootloader: GRUB2 (Linux), Windows Boot Manager, multi-boot configurations
   - Kernel loading: initramfs/initrd, kernel parameters, module loading
   - Init system: SysVinit → Upstart → systemd, PID 1 responsibilities
   - Login: getty, PAM, display manager (graphical) vs. TTY (console)
5. **Virtualization and emulation**
   - Type 1 hypervisors (bare-metal): ESXi, Hyper-V, Xen, KVM
   - Type 2 hypervisors (hosted): VirtualBox, VMware Workstation, Parallels, UTM
   - Hardware-assisted virtualization: ring -1, VMCS, VMEXIT/VMENTER
   - Paravirtualization: virtio drivers, why they improve performance
   - Emulation vs. virtualization: QEMU in emulation mode vs. KVM-accelerated
   - Containers vs. VMs: a preview (detailed in Section 13)

### Key Concepts

- The fetch-decode-execute cycle is the heartbeat of every CPU
- Memory hierarchy exists because physics forces a speed-capacity tradeoff
- The kernel is the boundary between hardware and everything else
- UEFI replaced BIOS; GPT replaced MBR — know why
- Virtualization extensions let one CPU pretend to be many CPUs

### Essential Commands and Tools

- `lscpu` — display CPU architecture info
- `free -h` — show memory usage
- `lsblk` — list block devices
- `dmidecode` — dump hardware info from BIOS/UEFI tables
- `dmesg` — kernel ring buffer (boot messages)
- `lspci` — list PCI devices
- `lsusb` — list USB devices
- `hdparm` — get/set SATA device parameters
- `smartctl` — SMART disk health

### Exercises

- Identify every hardware component in your learning machine (physical or virtual) using command-line tools
- Convert a set of numbers between binary, decimal, and hex by hand — then verify with `printf` or `python3`
- Trace the boot process of your Linux VM by reading `dmesg` output and matching each phase to the outline above
- Compare BIOS vs. UEFI boot on your system: check partition table type, identify bootloader, examine ESP
- Explain the memory hierarchy to a rubber duck, including approximate latency at each layer

### Proficiency Criteria

You can draw the full boot sequence from power-on to login prompt without notes. You can explain cache coherence to someone who has never heard of it. Given a hardware spec sheet, you can identify bottlenecks and explain why they matter. You understand why virtualization extensions exist and what they do at the CPU level.

---

## Section 3: OS Fundamentals

- **Position**: 3
- **Icon**: cpu
- **Prerequisites**: Section 2

### Learning Objectives

1. You can explain what an operating system kernel does and name its core responsibilities.
2. You can describe process lifecycle (creation, states, scheduling, termination) and explain how `fork()` and `exec()` work.
3. You can explain scheduling algorithms (round-robin, priority, CFS) and their tradeoffs.
4. You can describe virtual memory, paging, page tables, TLB, and swap — and why overcommit exists.
5. You can explain file system concepts: inodes, dentries, superblocks, journaling, and the VFS layer.
6. You can compare file systems (ext4, XFS, Btrfs, ZFS, NTFS) and explain when to use each.
7. You can describe the Unix permission model (DAC) and explain UIDs, GIDs, umask, setuid, setgid, sticky bit.
8. You can explain cgroups v2 and namespaces — the building blocks of containers.
9. You can describe how system calls bridge user space and kernel space.
10. You can explain what kernel modules are and how they extend the kernel without recompilation.
11. You can describe seccomp and explain how it restricts system calls.
12. You can explain inter-process communication mechanisms (pipes, sockets, shared memory, message queues, signals).

### Topic Outline

1. **What an operating system does**
   - Abstraction: hiding hardware complexity behind clean interfaces
   - Resource management: CPU time, memory, I/O, storage
   - Isolation: protecting processes from each other
   - Kernel architectures: monolithic (Linux), microkernel (Minix, QNX), hybrid (Windows NT, macOS/XNU)
   - Kernel space vs. user space: ring 0 vs. ring 3, system call boundary
2. **Processes and threads**
   - Process: PID, PPID, address space, file descriptors, environment
   - `fork()`: creating a child process (copy-on-write optimization)
   - `exec()`: replacing the current process image
   - `fork()`+`exec()` pattern: how shells launch programs
   - Process states: running, sleeping (interruptible/uninterruptible), stopped, zombie
   - Zombie processes: what they are, why they happen, how to clean them up
   - Orphan processes: reparenting to PID 1
   - Threads: shared address space, POSIX threads (pthreads), kernel threads vs. user threads
   - Thread safety: mutexes, semaphores, race conditions, deadlocks
   - Signals: SIGTERM, SIGKILL, SIGHUP, SIGINT, SIGCHLD — signal handling and traps
3. **CPU scheduling**
   - Preemptive vs. cooperative scheduling
   - Scheduling algorithms: FIFO, round-robin, priority-based, multilevel feedback queue
   - Linux CFS (Completely Fair Scheduler): virtual runtime, red-black tree, nice values, weight
   - Real-time scheduling: SCHED_FIFO, SCHED_RR, SCHED_DEADLINE
   - CPU affinity: pinning processes to cores, `taskset`
   - Context switching: what it costs, when it happens
   - Load average: what the three numbers mean, how to interpret them
4. **Memory management**
   - Physical vs. virtual memory: why the abstraction exists
   - Paging: page size (4KB default, huge pages 2MB/1GB), page tables, multi-level page tables
   - TLB (Translation Lookaside Buffer): caching page table entries, TLB flushes
   - Page faults: minor (page in memory, not mapped) vs. major (page on disk)
   - Swap: swap partitions vs. swap files, swappiness, zswap, zram
   - OOM killer: when it triggers, how it scores processes, `oom_score_adj`
   - Memory overcommit: what it is, `/proc/sys/vm/overcommit_memory` settings
   - Huge pages: transparent huge pages (THP) vs. explicit huge pages, when they help
   - Memory-mapped files: `mmap()`, shared memory
   - NUMA awareness: memory allocation policies, `numactl`
5. **File systems**
   - What a file system does: map names to data on disk
   - Inodes: metadata (permissions, timestamps, size, block pointers), inode table, inode exhaustion
   - Dentries: directory entries, the dcache
   - Superblock: file system metadata, mount options
   - Journaling: write-ahead logging, journal modes (writeback, ordered, journal)
   - File system types:
     - ext4: default Linux, mature, good general-purpose
     - XFS: high-performance, good for large files, online resize (grow only)
     - Btrfs: copy-on-write, snapshots, subvolumes, checksums, RAID
     - ZFS: copy-on-write, pools, datasets, send/receive, ARC cache (note: licensing issues on Linux)
     - NTFS: Windows, POSIX compatibility layer, large file support
     - tmpfs: RAM-backed, volatile
     - procfs, sysfs: virtual file systems exposing kernel data
   - VFS (Virtual File System): the abstraction layer that lets Linux support multiple file systems
   - Hard links vs. symbolic links: inode sharing vs. path pointers, when each breaks
   - File descriptors: stdin(0), stdout(1), stderr(2), `/proc/self/fd`, file descriptor limits
6. **Permissions and access control**
   - Unix DAC: owner, group, others — read, write, execute
   - Numeric (octal) vs. symbolic notation
   - `umask`: default permission mask, how to calculate resulting permissions
   - Special permissions: setuid (run as owner), setgid (inherit group), sticky bit (restrict deletion)
   - ACLs (Access Control Lists): `getfacl`, `setfacl`, when DAC is not granular enough
   - Linux capabilities: breaking root into fine-grained permissions (`CAP_NET_BIND_SERVICE`, `CAP_SYS_ADMIN`, etc.)
   - SELinux and AppArmor: MAC (Mandatory Access Control) overview, contexts/profiles, enforcing vs. permissive
7. **Namespaces and cgroups**
   - Linux namespaces: PID, network, mount, UTS, IPC, user, cgroup — what each isolates
   - How `unshare` and `nsenter` work
   - cgroups v2: unified hierarchy, controllers (cpu, memory, io, pids)
   - Resource limits: CPU shares/quota, memory limits (soft/hard), IO weight
   - Why namespaces + cgroups = containers (preview)
8. **System calls and kernel modules**
   - System calls: the API between user space and kernel space
   - Common syscalls: `open`, `read`, `write`, `close`, `fork`, `exec`, `mmap`, `ioctl`
   - `strace`: tracing system calls, interpreting output
   - Kernel modules: `lsmod`, `modprobe`, `insmod`, `rmmod`, `modinfo`
   - `/proc` and `/sys`: virtual filesystems for kernel introspection
   - seccomp: restricting available syscalls, seccomp-bpf profiles
9. **Inter-process communication**
   - Pipes: anonymous pipes (`|`), named pipes (FIFOs)
   - Unix domain sockets: local IPC, `AF_UNIX`
   - Shared memory: `shmget`/`shmat`, POSIX shared memory (`shm_open`)
   - Message queues: System V vs. POSIX
   - Signals: asynchronous notification, default handlers
   - D-Bus: desktop and systemd IPC

### Key Concepts

- Everything in Unix is a file (or pretends to be)
- `fork()`+`exec()` is how every process starts
- Virtual memory means every process thinks it has the entire address space
- The scheduler decides fairness; CFS uses virtual runtime to approximate it
- Namespaces isolate what a process can see; cgroups limit what it can use

### Essential Commands and Tools

- `ps aux` — list all processes
- `top` / `htop` — interactive process monitor
- `kill` / `kill -9` — send signals to processes
- `strace` — trace system calls
- `lsof` — list open files and file descriptors
- `df -h` — disk space usage
- `du -sh` — directory size
- `mount` — show mounted file systems
- `chmod` / `chown` / `chgrp` — modify permissions and ownership
- `getfacl` / `setfacl` — ACL management
- `ulimit` — view/set resource limits
- `sysctl` — view/set kernel parameters
- `lsmod` / `modprobe` — kernel module management
- `unshare` / `nsenter` — namespace operations

### Exercises

- Use `strace` to trace a simple command (`ls`) and identify the system calls involved
- Create a zombie process intentionally, observe it with `ps`, then clean it up
- Set up cgroups v2 to limit a process to 50% of one CPU core and 256MB of RAM — verify the limits work
- Create a file, hard link it, symlink it — then delete the original and observe what survives
- Configure ACLs on a directory to give a specific user read access without changing group ownership
- Use `unshare` to create a new PID namespace and observe that PID 1 inside is your shell

### Proficiency Criteria

You can explain the process lifecycle from `fork()` to `exit()` including every state transition. You can describe how CFS decides which process runs next. Given a system under memory pressure, you can explain what the OOM killer does and how to influence its behavior. You can set up namespace and cgroup isolation by hand. You understand that containers are just namespaces + cgroups + a filesystem image.

---

## Section 4: Linux

- **Position**: 4
- **Icon**: terminal
- **Prerequisites**: Section 3

### Learning Objectives

1. You can navigate the Linux filesystem hierarchy and explain the purpose of every top-level directory.
2. You can perform all file operations (create, copy, move, rename, delete, find, search) fluently from the terminal.
3. You can manage users, groups, and sudo configuration.
4. You can install, update, remove, and query packages across multiple package managers (apt, dnf, pacman).
5. You can manage systemd services: start, stop, enable, disable, create custom units, read journal logs.
6. You can configure and troubleshoot networking from the command line (ip, ss, nmcli, hostnamectl).
7. You can use /proc and /sys to inspect and tune system behavior at runtime.
8. You can profile system performance using built-in tools (vmstat, iostat, sar, perf basics).
9. You can explain and configure PAM authentication modules.
10. You can work with disk partitions, file systems, and mount points (fdisk/parted, mkfs, fstab).
11. You can use `find` with complex predicates and actions, and know when to use `locate` instead.
12. You can manage environment variables, shell configuration files, and login vs. non-login shell differences.

### Topic Outline

1. **The Linux filesystem hierarchy**
   - FHS (Filesystem Hierarchy Standard): what it defines and why
   - `/` — root, `/bin`, `/sbin` — essential binaries
   - `/etc` — system configuration, `/var` — variable data (logs, mail, spool)
   - `/home` — user home directories, `/root` — root's home
   - `/tmp` — temporary files (cleared on reboot), `/run` — runtime data
   - `/usr` — user programs (`/usr/bin`, `/usr/lib`, `/usr/share`), the usr merge
   - `/opt` — optional/third-party software
   - `/dev` — device files (block, character), `/dev/null`, `/dev/zero`, `/dev/urandom`
   - `/proc` — process and kernel info (virtual), `/sys` — device and driver info (virtual)
   - `/mnt` and `/media` — mount points
   - `/boot` — kernel, initramfs, bootloader files
2. **File operations mastery**
   - Basics: `ls`, `cp`, `mv`, `rm`, `mkdir`, `touch`, `cat`, `less`, `more`, `head`, `tail`
   - `ls` deep dive: `-l` (long), `-a` (hidden), `-R` (recursive), `-h` (human-readable), `-i` (inodes), `-S` (sort by size), `-t` (sort by time)
   - Wildcards and globbing: `*`, `?`, `[abc]`, `[a-z]`, `{foo,bar}`, extended globbing
   - `find`: by name, type, size, mtime, user, permissions, `-exec`, `-delete`, `-print0` | `xargs -0`
   - `locate` / `mlocate` / `plocate`: updatedb, when to use over find
   - `stat`: detailed file metadata
   - `file`: determine file type by content (magic bytes), not extension
   - `ln`: hard links, symbolic links, relative vs. absolute symlink targets
   - `tar`: create, extract, list, compression (gzip, bzip2, xz, zstd), common flags
   - `rsync`: local and remote sync, `--archive`, `--delete`, `--dry-run`, `--exclude`
   - `dd`: disk duplication, creating disk images, block size tuning, `status=progress`
3. **Text processing pipeline**
   - `grep`: basic and extended regex, `-r` (recursive), `-l` (files only), `-c` (count), `-v` (invert), `-i` (case-insensitive)
   - `sed`: substitution (`s///`), deletion (`d`), in-place (`-i`), address ranges
   - `awk`: field processing, `$1`, `$NF`, `BEGIN`/`END` blocks, pattern matching
   - `sort`: numeric (`-n`), reverse (`-r`), field (`-k`), unique (`-u`)
   - `uniq`: deduplicate (requires sorted input), `-c` (count)
   - `cut`: extract fields by delimiter or character position
   - `tr`: translate/delete characters
   - `wc`: count lines, words, bytes
   - `diff` / `colordiff` / `vimdiff`: comparing files
   - `tee`: split output to file and stdout simultaneously
   - Combining tools: multi-stage pipelines for data extraction and transformation
4. **User and group management**
   - `/etc/passwd`: format, fields, system vs. human users
   - `/etc/shadow`: password hashes, aging, account locking
   - `/etc/group`: group membership, primary vs. supplementary groups
   - `useradd` / `adduser`: creating users, home directory, shell, UID/GID assignment
   - `usermod`: modify user properties, add to groups (`-aG`), lock/unlock
   - `userdel`: remove users, `-r` to remove home directory
   - `groupadd` / `groupmod` / `groupdel`: group management
   - `passwd`: change passwords, enforce policies
   - `su` vs. `sudo`: switching users vs. executing as another user
   - `visudo` and `/etc/sudoers`: syntax, NOPASSWD, command restrictions, sudoers.d
   - PAM (Pluggable Authentication Modules): how authentication is modular, common modules (`pam_unix`, `pam_pwquality`, `pam_limits`), configuration files in `/etc/pam.d/`
5. **Package management**
   - Concepts: repositories, dependencies, package metadata, signing/verification
   - apt (Debian/Ubuntu): `apt update`, `apt install`, `apt remove`, `apt purge`, `apt search`, `apt show`, `apt list --installed`, `/etc/apt/sources.list`, PPAs
   - dnf (Fedora/RHEL): `dnf install`, `dnf remove`, `dnf search`, `dnf info`, `dnf list`, `dnf history`, COPR repos
   - pacman (Arch): `pacman -S`, `pacman -R`, `pacman -Ss`, `pacman -Qi`, `pacman -Syu`, AUR and AUR helpers
   - Package formats: `.deb`, `.rpm`, `.pkg.tar.zst`
   - Low-level tools: `dpkg`, `rpm` — when and why to use them
   - Universal packages: Snap, Flatpak, AppImage — tradeoffs (isolation vs. size vs. integration)
   - Building from source: `./configure && make && make install`, `checkinstall`, when to do it
6. **systemd**
   - What systemd is: init system, service manager, and more
   - Unit files: service, timer, socket, mount, target — anatomy of a unit file
   - `systemctl`: start, stop, restart, reload, enable, disable, status, is-active, is-enabled
   - `systemctl list-units`, `list-unit-files`, `daemon-reload`
   - Writing custom service units: `[Unit]`, `[Service]`, `[Install]` sections, `ExecStart`, `Restart`, `WantedBy`
   - Service types: simple, forking, oneshot, notify, idle
   - Targets: multi-user.target, graphical.target, rescue.target — equivalent of runlevels
   - Timers: replacement for cron, monotonic vs. realtime, `OnCalendar` syntax
   - `journalctl`: reading logs, `-u` (unit), `-f` (follow), `--since`, `--until`, `-p` (priority), persistent logging configuration
   - `systemd-analyze`: boot time analysis, blame, critical-chain
7. **Disk management**
   - Partitioning: `fdisk` (MBR), `gdisk` (GPT), `parted` (both)
   - File system creation: `mkfs.ext4`, `mkfs.xfs`, `mkfs.btrfs`
   - Mounting: `mount`, `umount`, `/etc/fstab` format, mount options (noexec, nosuid, nodev, ro)
   - UUID vs. device path: why UUIDs are preferred, `blkid`
   - LVM (Logical Volume Manager): PV, VG, LV concepts, creating and resizing volumes, snapshots
   - RAID: levels (0, 1, 5, 6, 10), `mdadm` basics, software vs. hardware RAID
   - Swap management: `mkswap`, `swapon`, `swapoff`, swap files vs. partitions
   - Disk monitoring: `smartctl`, `iostat`, `iotop`
8. **Networking on Linux**
   - `ip`: addresses (`ip addr`), routes (`ip route`), links (`ip link`), neighbors (`ip neigh`)
   - `ss`: socket statistics, replacement for `netstat`, filtering by state/port/protocol
   - `nmcli` / `nmtui`: NetworkManager CLI/TUI for connection management
   - `/etc/hosts`, `/etc/resolv.conf`, `/etc/nsswitch.conf`: name resolution config
   - `hostnamectl`: set hostname, pretty hostname, static hostname
   - `ping`, `traceroute`/`tracepath`, `mtr`: connectivity testing
   - `dig` / `nslookup`: DNS queries
   - `curl` / `wget`: HTTP requests and file downloads
   - Firewall: `iptables` basics (tables, chains, rules), `nftables` as successor, `ufw`/`firewalld` as frontends
9. **Performance analysis**
   - `vmstat`: virtual memory statistics, procs/memory/swap/io/system/cpu columns
   - `iostat`: disk I/O statistics, average queue length, utilization
   - `sar`: historical system activity (CPU, memory, disk, network), `sysstat` package
   - `mpstat`: per-CPU statistics
   - `pidstat`: per-process resource usage
   - `perf`: basic profiling — `perf stat`, `perf top`, `perf record` + `perf report`
   - `/proc` deep dive: `/proc/cpuinfo`, `/proc/meminfo`, `/proc/loadavg`, `/proc/[pid]/status`, `/proc/[pid]/maps`, `/proc/[pid]/fd`
   - `/sys` deep dive: `/sys/class`, `/sys/block`, `/sys/fs/cgroup`
   - Kernel parameters: `sysctl`, `/etc/sysctl.conf`, common tuning (`net.core.somaxconn`, `vm.swappiness`, `fs.file-max`)
10. **Environment and shell configuration**
    - Environment variables: `export`, `env`, `printenv`, `PATH`, `HOME`, `USER`, `SHELL`, `LANG`
    - Shell startup files: `/etc/profile`, `~/.bash_profile`, `~/.bashrc`, `~/.profile` — login vs. non-login, interactive vs. non-interactive
    - `source` (`.`): reload configuration without restarting shell
    - Shell options: `set -e`, `set -u`, `set -o pipefail`, `shopt`
    - Aliases and functions: defining, persisting, when to use each
    - `XDG_*` directories: config, data, cache, runtime — the Base Directory Specification

### Key Concepts

- The FHS gives every file a predictable home
- `/proc` and `/sys` are windows into the running kernel — not real files
- systemd is PID 1 and manages the entire service lifecycle
- Package managers handle dependency resolution so you do not have to
- Performance analysis starts with knowing which tool shows which metric

### Essential Commands and Tools

- `ls`, `cp`, `mv`, `rm`, `mkdir`, `find`, `grep`, `sed`, `awk` — file and text operations
- `useradd`, `usermod`, `passwd`, `visudo` — user management
- `apt` / `dnf` / `pacman` — package management
- `systemctl`, `journalctl` — service and log management
- `ip`, `ss`, `dig`, `curl` — networking
- `fdisk`, `mkfs`, `mount`, `blkid`, `lvm` — disk management
- `vmstat`, `iostat`, `sar`, `perf` — performance

### Exercises

- Navigate to every top-level FHS directory and explain what you find in each
- Build a multi-stage text processing pipeline: extract, filter, sort, and summarize data from a log file
- Create a user, configure sudo with specific command restrictions, verify the restrictions work
- Install the same package on two different distros (apt and dnf) and compare the experience
- Write a custom systemd service unit that runs a script, configure it to restart on failure, enable it at boot
- Partition a disk with GPT, create an ext4 filesystem, mount it persistently via fstab using UUID
- Profile a running process with `perf stat` and interpret the output
- Tune a kernel parameter with `sysctl`, verify the change, and make it persistent

### Proficiency Criteria

You move through the Linux terminal without hesitation. You can manage users, packages, services, disks, and networking entirely from the command line on any major distribution. You use `/proc` and `/sys` to investigate system behavior. You know which performance tool to reach for based on the symptom. You can write systemd unit files from memory.

---

## Section 5: Text Editing

- **Position**: 5
- **Icon**: pen-tool
- **Prerequisites**: Section 4

### Learning Objectives

1. You can open, edit, save, and quit files in Vim without hesitation.
2. You can explain Vim's modal editing model and switch between normal, insert, visual, and command-line modes fluently.
3. You can navigate files using motions (word, sentence, paragraph, search, marks) without arrow keys.
4. You can use registers (unnamed, named, system clipboard, expression) for complex copy/paste workflows.
5. You can record and replay macros to automate repetitive edits.
6. You can use Vim's built-in search and replace with regular expressions.
7. You can configure Vim via `.vimrc` with settings, mappings, and autocommands.
8. You can use multiple buffers, windows, and tabs to edit several files simultaneously.
9. You can explain the Unix philosophy of text-based tools and why terminal text editors matter.
10. You can use `sed` and `awk` for non-interactive editing and data transformation.

### Topic Outline

1. **Why terminal text editors matter**
   - The Unix philosophy: text as a universal interface
   - Every server has `vi` — it is everywhere, always available
   - SSH + terminal editor = editing files on any machine
   - Vim vs. Nano vs. Emacs: different philosophies, why we focus on Vim
   - Alternative editors: Neovim, Helix, Micro — the landscape
2. **Vim's modal model**
   - Normal mode: the default, where you think and navigate
   - Insert mode: where you type text (`i`, `I`, `a`, `A`, `o`, `O`, `c`, `s`)
   - Visual mode: character (`v`), line (`V`), block (`Ctrl-v`) selection
   - Command-line mode: `:` commands, `/` search, `!` shell commands
   - Replace mode: `R` for overwrite editing
   - Why modes? The efficiency argument: most time is spent reading and navigating, not typing
3. **Navigation (motions)**
   - Character: `h`, `j`, `k`, `l`
   - Word: `w`, `b`, `e`, `W`, `B`, `E` (WORD vs. word)
   - Line: `0`, `^`, `$`, `g_`
   - Screen: `H`, `M`, `L`, `Ctrl-u`, `Ctrl-d`, `Ctrl-f`, `Ctrl-b`, `zt`, `zz`, `zb`
   - File: `gg`, `G`, `:{line}`
   - Search: `/pattern`, `?pattern`, `n`, `N`, `*`, `#`
   - Marks: `ma` (set), `'a` (jump to line), `` `a `` (jump to position), `''` (last position)
   - Jumps: `Ctrl-o`, `Ctrl-i`, jump list, change list (`g;`, `g,`)
   - Paragraphs and sentences: `{`, `}`, `(`, `)`
4. **Operators and text objects**
   - Operators: `d` (delete), `c` (change), `y` (yank), `>` (indent), `<` (dedent), `=` (format), `gU`/`gu` (case)
   - The operator + motion grammar: `d3w`, `ciw`, `y$`, `>}`
   - Text objects: `iw` (inner word), `aw` (a word), `i"` (inner quotes), `a(` (a parenthetical), `it` (inner tag), `ip` (inner paragraph)
   - The verb-noun composability: operators are verbs, motions/text objects are nouns
   - `.` (dot): repeat the last change — the most powerful key in Vim
   - Counts: `3dd`, `5j`, `2dw` — numeric prefixes multiply actions
5. **Registers**
   - Unnamed register (`""`): default yank/delete destination
   - Named registers (`"a` through `"z`): explicit storage, uppercase appends
   - System clipboard: `"+` and `"*`, interaction with OS clipboard
   - Expression register (`"=`): evaluate expressions inline
   - Read-only registers: `"%` (filename), `"#` (alternate file), `".` (last insert), `":` (last command)
   - Black hole register (`"_`): delete without affecting registers
   - Numbered registers (`"0` through `"9`): yank and delete history
6. **Search and replace**
   - `/pattern` and `?pattern`: forward and backward search
   - Regular expressions in Vim: `\v` (very magic), character classes, quantifiers, groups, backreferences
   - `:s/old/new/` — substitute in current line
   - `:%s/old/new/g` — substitute in entire file
   - Flags: `g` (global), `c` (confirm), `i` (case-insensitive), `n` (count only)
   - `:g/pattern/command` — global command: execute on matching lines
   - `:v/pattern/command` — inverse global: execute on non-matching lines
7. **Macros**
   - Recording: `qa` (start recording to register a), `q` (stop)
   - Playback: `@a` (play register a), `@@` (repeat last macro), `10@a` (play 10 times)
   - Recursive macros: macro that calls itself
   - Editing macros: paste register, edit, yank back
   - Practical use cases: reformatting data, repetitive code changes, batch operations
8. **Buffers, windows, and tabs**
   - Buffers: `:ls`, `:b{n}`, `:bnext`, `:bprev`, `:bd` — every open file is a buffer
   - Windows (splits): `:split`, `:vsplit`, `Ctrl-w` commands (`h/j/k/l`, `=`, `_`, `|`, `o`)
   - Tabs: `:tabnew`, `:tabnext`, `:tabprev`, `gt`, `gT` — tab pages contain window layouts
   - Argument list: `:args`, `:argdo`, batch operations across files
   - Quickfix list: `:copen`, `:cnext`, `:cprev` — navigating search results and compiler errors
9. **Configuration (.vimrc)**
   - Settings: `set number`, `set relativenumber`, `set expandtab`, `set shiftwidth=2`, `set ignorecase`, `set smartcase`, `set incsearch`, `set hlsearch`
   - Key mappings: `nnoremap`, `inoremap`, `vnoremap`, `<leader>` key
   - Autocommands: `autocmd FileType`, `autocmd BufWritePre` — event-driven configuration
   - File type detection and filetype plugins
   - Status line customization
   - Neovim differences: `init.lua`, Lua configuration, built-in LSP
10. **Non-interactive text editing**
    - `sed`: stream editing for automation — when you need to edit files in scripts
    - `awk`: field-based text processing — when you need structured data extraction
    - `ed`: the original line editor — understanding the heritage
    - `envsubst`, `m4`, `jinja2`: template-based text generation
    - When to use Vim vs. sed vs. awk vs. a script

### Key Concepts

- Modal editing: separate modes for thinking (normal), typing (insert), and selecting (visual)
- Operators + motions/text objects = a composable editing language
- `.` repeats the last change — design your edits to be repeatable
- Registers are Vim's clipboard system — there are many of them, learn to use them
- Macros turn one correct edit into a hundred correct edits

### Essential Commands and Tools

- `vim` / `vi` — the editor
- `vimtutor` — built-in interactive tutorial
- `sed` — stream editor
- `awk` — pattern-directed text processing
- `vimdiff` — side-by-side diff editing

### Exercises

- Complete `vimtutor` in under 20 minutes
- Open a file, navigate to a specific function using only motions (no arrow keys, no mouse), make a change, save and quit
- Use a macro to convert 50 lines of CSV into a different format
- Configure a `.vimrc` from scratch with your preferred settings, at least 5 custom mappings
- Use `:%s` with regex to perform a complex find-and-replace across a codebase file
- Edit three files simultaneously using splits and buffers, copy text between them using named registers
- Use `:g/pattern/d` to delete all lines matching a pattern, then `:v/pattern/d` to keep only matching lines

### Proficiency Criteria

You edit files in Vim at the speed of thought. You never reach for arrow keys. You compose operators and motions naturally (`ciw`, `da(`, `yi"`). You record macros without hesitation. You can configure Vim for any workflow. You understand that Vim's modal model is a language, not a set of shortcuts to memorize.

---

## Section 6: Shell Scripting

- **Position**: 6
- **Icon**: file-code
- **Prerequisites**: Sections 4, 5

### Learning Objectives

1. You can write Bash scripts that accept arguments, validate input, and handle errors gracefully.
2. You can use variables, arrays, associative arrays, and parameter expansion fluently.
3. You can write conditional logic (if/elif/else, case, test expressions) for any scenario.
4. You can write loops (for, while, until, select) including iterating over files, lines, and command output.
5. You can use pipes, redirection (stdin/stdout/stderr), process substitution, and here documents.
6. You can write functions with local variables, return values, and proper scoping.
7. You can trap signals and implement cleanup handlers for robust scripts.
8. You can schedule recurring tasks with cron and systemd timers.
9. You can debug scripts using `set -x`, `set -e`, `set -u`, `set -o pipefail`, and `bash -x`.
10. You can parse structured data (CSV, JSON, logs) using shell tools and know when to switch to Python.
11. You can write scripts that are portable across Bash versions and POSIX-compliant when needed.
12. You can explain the difference between subshells and child processes and their implications.

### Topic Outline

1. **Script fundamentals**
   - Shebang: `#!/bin/bash` vs. `#!/usr/bin/env bash` — portability considerations
   - Script execution: `chmod +x`, `./script.sh`, `bash script.sh`, `source script.sh` — differences
   - Exit codes: `$?`, `exit 0`, `exit 1`, convention (0 = success, non-zero = failure)
   - Comments: `#` inline, no multi-line comment syntax (use `: '...'` or heredoc trick)
   - Script structure: argument parsing, validation, main logic, cleanup
   - Shellcheck: static analysis for shell scripts, common pitfalls it catches
2. **Variables and data types**
   - Variable assignment: `VAR=value` (no spaces around `=`)
   - Quoting: single quotes (literal), double quotes (interpolation), no quotes (word splitting + glob expansion)
   - Parameter expansion: `${VAR}`, `${VAR:-default}`, `${VAR:=default}`, `${VAR:?error}`, `${VAR:+alt}`
   - String operations: `${#VAR}` (length), `${VAR#pattern}` (trim prefix), `${VAR%pattern}` (trim suffix), `${VAR/old/new}` (replace), `${VAR^^}` (uppercase), `${VAR,,}` (lowercase)
   - Arrays: `arr=(a b c)`, `${arr[0]}`, `${arr[@]}`, `${#arr[@]}`, `arr+=(d)`, iteration
   - Associative arrays: `declare -A map`, `map[key]=value`, iteration over keys and values
   - Arithmetic: `$(( ))`, `let`, integer-only (use `bc` or `awk` for floating-point)
   - Special variables: `$0` (script name), `$1`-`$9` (positional), `$#` (count), `$@` vs. `$*`, `$$` (PID), `$!` (last background PID), `$_` (last argument)
   - `readonly` and `local`: immutable variables, function-scoped variables
3. **Conditionals and test expressions**
   - `if`/`elif`/`else`/`fi`: basic branching
   - `test` / `[ ]`: POSIX test — string, integer, and file tests
   - `[[ ]]`: Bash extended test — pattern matching (`==`, `=~`), logical operators (`&&`, `||`), no word splitting
   - String tests: `-z` (empty), `-n` (non-empty), `==`, `!=`
   - Integer tests: `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`
   - File tests: `-f` (regular file), `-d` (directory), `-e` (exists), `-r`/`-w`/`-x` (permissions), `-s` (non-empty), `-L` (symlink)
   - `case` statements: pattern matching with `|` (OR), `*` (default), `;;` (break), `;&` (fallthrough)
   - Short-circuit: `cmd1 && cmd2`, `cmd1 || cmd2`
4. **Loops and iteration**
   - `for var in list`: iterate over words, files, command output, brace expansion `{1..100}`
   - C-style for: `for ((i=0; i<10; i++))`
   - `while read -r line`: process input line by line (with `IFS` considerations)
   - `while true` / `until`: indefinite loops with break conditions
   - `select`: menu-driven user interaction
   - Loop control: `break`, `continue`, `break N` (nested loops)
   - Looping over files safely: handling spaces, special characters, empty glob results (`nullglob`)
5. **I/O: pipes, redirection, and process substitution**
   - File descriptors: 0 (stdin), 1 (stdout), 2 (stderr)
   - Output redirection: `>` (overwrite), `>>` (append), `2>` (stderr), `&>` (both), `2>&1` (merge stderr into stdout)
   - Input redirection: `<`, here documents (`<<EOF`), here strings (`<<<`)
   - Pipes: `|`, pipeline exit status (`PIPESTATUS`), `set -o pipefail`
   - Process substitution: `<(cmd)` and `>(cmd)` — treat command output as a file
   - `tee`: duplicate output to file and stdout
   - `/dev/null`: discard output
   - Named pipes (FIFOs): `mkfifo`, inter-process communication
   - `exec`: redirect file descriptors for the entire script, open/close FDs
6. **Functions**
   - Declaration: `function name { }` vs. `name() { }`
   - Arguments: `$1`, `$2`, `$@`, `$#` — same as script arguments but scoped to the function
   - Return values: `return N` (exit code only, 0-255), stdout capture for data (`result=$(func)`)
   - Local variables: `local var=value`, scoping rules (dynamic scoping in Bash)
   - Libraries: `source lib.sh`, organizing reusable functions
   - Recursion: possible but watch the stack (no tail-call optimization)
7. **Error handling and debugging**
   - `set -e` (errexit): exit on any non-zero return
   - `set -u` (nounset): error on undefined variables
   - `set -o pipefail`: pipeline fails if any command fails
   - `set -x` (xtrace): print every command before execution
   - The defensive preamble: `set -euo pipefail`
   - `trap`: catch signals (EXIT, ERR, INT, TERM), implement cleanup handlers
   - `trap cleanup EXIT`: guaranteed cleanup pattern
   - Error messages: write to stderr (`echo "error" >&2`)
   - `bash -x script.sh`: debug without modifying the script
   - `PS4='+ ${BASH_SOURCE}:${LINENO}: '`: better debug output with file and line info
8. **Scheduling**
   - Cron: crontab syntax (minute, hour, day, month, weekday), `crontab -e`, `crontab -l`
   - Cron special strings: `@reboot`, `@hourly`, `@daily`, `@weekly`, `@monthly`
   - Cron environment: limited `PATH`, no TTY, stdout goes to mail
   - Cron pitfalls: overlapping runs, missing environment, timezone issues
   - systemd timers: `OnCalendar`, `OnBootSec`, `OnUnitActiveSec`, advantages over cron (logging, dependencies, resource control)
   - `at`: one-time scheduled execution
   - Locking: `flock` to prevent concurrent execution of the same script
9. **Practical patterns**
   - Argument parsing: `getopts` (POSIX), manual `while`+`case` for long options, `shift`
   - Configuration files: sourcing `.env` files, parsing key=value, `envsubst`
   - Temporary files: `mktemp`, `mktemp -d`, cleanup with `trap`
   - Logging: timestamped output, log levels, writing to syslog (`logger`)
   - Lock files: `flock`, PID files, preventing duplicate execution
   - Working with JSON: `jq` basics (`.key`, `.[0]`, pipes, `select`, `map`, `@csv`)
   - Working with CSV: `cut`, `awk`, `column -t`, when shell tools are enough vs. when to use Python
   - POSIX vs. Bash: what `#!/bin/sh` means, which features are Bash-only (`[[ ]]`, arrays, `{1..10}`, process substitution)

### Key Concepts

- `set -euo pipefail` is the defensive preamble — use it in every script
- Shell scripts glue other programs together — they are not a general-purpose programming language
- Quoting prevents word splitting and glob expansion — when in doubt, quote
- Functions return exit codes; use stdout capture for data
- `trap EXIT` guarantees cleanup even when scripts fail

### Essential Commands and Tools

- `bash` — the shell and script interpreter
- `shellcheck` — static analysis for shell scripts
- `jq` — JSON processor
- `crontab` — manage cron schedules
- `flock` — file-based locking
- `mktemp` — create temporary files/directories
- `getopts` — parse command-line options
- `xargs` — build and execute commands from stdin
- `logger` — write to syslog

### Exercises

- Write a script that accepts flags (`-v` for verbose, `-o FILE` for output) using `getopts`, validates inputs, and handles errors with `trap EXIT`
- Write a script that processes a log file: extract timestamps, count unique IPs, find the top 10 most frequent, output as CSV
- Build a script that monitors a directory for new files and processes them (using `inotifywait` or polling)
- Create a cron job and an equivalent systemd timer, compare their behavior and logging
- Write a library of reusable functions (logging, argument validation, temp file management), source it from multiple scripts
- Debug a deliberately broken script using `set -x` and `PS4` until all issues are found
- Write a script that safely handles filenames with spaces, special characters, and empty glob results

### Proficiency Criteria

You write scripts with `set -euo pipefail` by default. You handle errors, clean up temporary resources, and validate input without thinking. You know when shell scripting is the right tool and when to reach for Python. Your scripts pass `shellcheck` without warnings. You can read and modify someone else's shell scripts confidently.

---

## Section 7: Programming

- **Position**: 7
- **Icon**: code
- **Prerequisites**: Section 6

### Learning Objectives

1. You can write Python programs with correct syntax, style (PEP 8), and idiomatic patterns.
2. You can use all built-in data structures (lists, tuples, dicts, sets) and choose the right one for each situation.
3. You can write functions with default arguments, *args, **kwargs, type hints, and docstrings.
4. You can use list comprehensions, generator expressions, and built-in functions (map, filter, zip, enumerate).
5. You can implement classes with inheritance, dunder methods, properties, and class methods.
6. You can handle exceptions properly: try/except/else/finally, custom exceptions, context managers.
7. You can read and write files, parse JSON/YAML/CSV, and work with structured data.
8. You can manage virtual environments and dependencies with pip, venv, and requirements.txt.
9. You can write and run tests with pytest, including fixtures, parametrize, and mocking.
10. You can use the standard library effectively: os, sys, pathlib, subprocess, argparse, logging, re, datetime, collections, itertools.
11. You can explain Python's execution model: interpreted, bytecode compilation, GIL, reference counting + gc.
12. You can structure a Python project with packages, modules, and entry points.

### Topic Outline

1. **Python fundamentals**
   - Installation: system Python vs. managed (pyenv, deadsnakes PPA, Fedora modules)
   - REPL: interactive exploration, `help()`, `dir()`, `type()`
   - Syntax: indentation-based blocks, no braces, no semicolons
   - Variables: dynamic typing, name binding, everything is an object
   - Numbers: int (arbitrary precision), float (IEEE 754), complex
   - Strings: f-strings, `.format()`, `.join()`, `.split()`, `.strip()`, `.replace()`, slicing, immutability
   - Booleans: `True`/`False`, truthy/falsy values, short-circuit evaluation
   - None: the null value, checking with `is None` (not `== None`)
   - Comments: `#` inline, docstrings (`"""..."""`) for functions/classes/modules
   - PEP 8: the style guide, why consistency matters, `black` and `ruff` formatters
2. **Data structures**
   - Lists: ordered, mutable, heterogeneous, `append()`, `extend()`, `insert()`, `pop()`, slicing, copying (shallow vs. deep)
   - Tuples: ordered, immutable, use for fixed collections, named tuples
   - Dicts: key-value mapping, O(1) lookup, `.get()`, `.setdefault()`, `.items()`, `.keys()`, `.values()`, dict comprehensions, `defaultdict`, `Counter`
   - Sets: unordered, unique elements, O(1) membership test, union, intersection, difference, symmetric difference
   - Choosing the right structure: access patterns, mutability needs, ordering guarantees
   - Deque: `collections.deque`, O(1) append/pop from both ends
   - Heapq: priority queue operations
3. **Control flow**
   - `if`/`elif`/`else`, ternary expression (`x if cond else y`)
   - `for` loops: iterating over sequences, `range()`, `enumerate()`, `zip()`
   - `while` loops, `break`, `continue`, `else` clause on loops
   - Match statement (Python 3.10+): structural pattern matching
   - Iterators and generators: `__iter__`/`__next__`, `yield`, generator expressions, lazy evaluation, memory efficiency
   - `itertools`: `chain`, `islice`, `groupby`, `product`, `permutations`, `combinations`
4. **Functions**
   - Definition: `def`, parameters, return values
   - Arguments: positional, keyword, default values, `*args`, `**kwargs`, keyword-only (`*` separator), positional-only (`/` separator)
   - Type hints: `int`, `str`, `list[int]`, `dict[str, Any]`, `Optional`, `Union`, `typing` module
   - Docstrings: Google style, NumPy style, Sphinx style — pick one and be consistent
   - First-class functions: passing functions as arguments, returning functions
   - Lambda: anonymous functions, when to use (sort keys, simple callbacks)
   - Closures: capturing variables from enclosing scope, `nonlocal`
   - Decorators: `@decorator` syntax, writing decorators, `functools.wraps`, common decorators (`@staticmethod`, `@classmethod`, `@property`)
   - Scope: LEGB rule (Local, Enclosing, Global, Built-in), `global` keyword
5. **Object-oriented programming**
   - Classes: `class`, `__init__`, `self`, instance attributes vs. class attributes
   - Methods: instance methods, `@classmethod`, `@staticmethod`
   - Dunder methods: `__repr__`, `__str__`, `__eq__`, `__hash__`, `__len__`, `__getitem__`, `__iter__`, `__enter__`/`__exit__`
   - Properties: `@property`, getter/setter/deleter, computed attributes
   - Inheritance: single, multiple, MRO (Method Resolution Order), `super()`
   - Abstract base classes: `abc.ABC`, `@abstractmethod`
   - Dataclasses: `@dataclass`, `field()`, `frozen=True`, when to use over regular classes
   - Protocols (structural subtyping): `typing.Protocol`, duck typing formalized
   - Composition vs. inheritance: when to use each
6. **Error handling**
   - Exceptions: `try`/`except`/`else`/`finally`
   - Built-in exceptions: `ValueError`, `TypeError`, `KeyError`, `FileNotFoundError`, `OSError`, hierarchy
   - Custom exceptions: subclassing `Exception`, adding context
   - EAFP vs. LBYL: "Easier to Ask Forgiveness than Permission" — the Pythonic approach
   - Context managers: `with` statement, `__enter__`/`__exit__`, `contextlib.contextmanager`
   - Exception chaining: `raise ... from ...`
   - Logging exceptions: `logging.exception()` captures traceback
7. **File I/O and data formats**
   - File operations: `open()`, `read()`, `write()`, `readlines()`, `with` for automatic closing
   - Modes: `r`, `w`, `a`, `rb`, `wb` — text vs. binary
   - `pathlib`: `Path` objects, `/` operator for joining, `.exists()`, `.is_file()`, `.is_dir()`, `.glob()`, `.read_text()`, `.write_text()`
   - JSON: `json.loads()`, `json.dumps()`, `json.load()`, `json.dump()`, custom serialization
   - YAML: `pyyaml`, `yaml.safe_load()`, `yaml.dump()` — why `safe_load` always
   - CSV: `csv.reader`, `csv.DictReader`, `csv.writer`, `csv.DictWriter`
   - TOML: `tomllib` (Python 3.11+), configuration files
   - INI: `configparser`, legacy configuration format
   - Encoding: always specify `encoding='utf-8'` explicitly
8. **Virtual environments and packages**
   - Why isolation matters: dependency conflicts, reproducibility, system Python protection
   - `venv`: `python -m venv .venv`, activation (`source .venv/bin/activate`), deactivation
   - `pip`: `pip install`, `pip install -r requirements.txt`, `pip freeze`, `pip list`
   - `requirements.txt`: pinning versions (`==`), compatible releases (`~=`), ranges
   - `pyproject.toml`: modern Python packaging, `[project]` metadata, `[build-system]`
   - `uv`: fast Python package installer and resolver (Rust-based alternative to pip)
   - Package managers comparison: pip, pipenv, poetry, uv — tradeoffs
9. **Testing**
   - Why test: regression prevention, design feedback, documentation
   - `pytest`: test discovery, `assert`, test functions, running tests
   - Fixtures: `@pytest.fixture`, setup/teardown, scope (function, class, module, session)
   - Parametrize: `@pytest.mark.parametrize`, testing multiple inputs
   - Mocking: `unittest.mock`, `patch`, `MagicMock`, when and why to mock
   - Coverage: `pytest-cov`, measuring test coverage, what coverage misses
   - Test organization: `tests/` directory, naming conventions, conftest.py
10. **Standard library highlights**
    - `os` and `sys`: environment, platform, interpreter info, exit
    - `pathlib`: modern path handling (prefer over `os.path`)
    - `subprocess`: `subprocess.run()`, capturing output, shell=True risks, `check=True`
    - `argparse`: CLI argument parsing, subcommands, help generation
    - `logging`: log levels, handlers, formatters, `basicConfig`, named loggers
    - `re`: regular expressions, `search`, `match`, `findall`, `sub`, compiled patterns
    - `datetime`: date, time, datetime, timedelta, timezone-aware vs. naive, `zoneinfo`
    - `collections`: `defaultdict`, `Counter`, `OrderedDict`, `namedtuple`, `deque`
    - `functools`: `lru_cache`, `partial`, `reduce`, `wraps`
    - `shutil`: file/directory operations, `copytree`, `rmtree`, `disk_usage`
11. **Python internals**
    - Execution model: source → bytecode (`.pyc`) → CPython VM
    - GIL (Global Interpreter Lock): what it is, why it exists, implications for threading
    - Memory: reference counting + generational garbage collector, `sys.getrefcount()`
    - Threading vs. multiprocessing: GIL limits threads for CPU-bound work, `concurrent.futures`
    - Async: `asyncio`, `async`/`await` — for IO-bound concurrency (preview, not deep dive)
12. **Project structure**
    - Modules: one `.py` file, `import`, `from ... import`, relative imports
    - Packages: directory with `__init__.py`, nested packages
    - Entry points: `if __name__ == '__main__':`, `__main__.py`, console scripts in `pyproject.toml`
    - Project layout: `src/` layout vs. flat layout, when each is appropriate
    - `.gitignore` for Python: `__pycache__/`, `.venv/`, `*.pyc`, `.env`

### Key Concepts

- Python is dynamically typed but you should use type hints for documentation and tooling
- Everything in Python is an object — functions, classes, modules, even types
- The `with` statement ensures cleanup — use it for files, locks, connections, anything with setup/teardown
- Virtual environments prevent dependency hell — one per project, always
- Test your code — `pytest` makes it frictionless

### Essential Commands and Tools

- `python3` — the interpreter
- `pip` / `uv` — package installer
- `python -m venv` — create virtual environments
- `pytest` — test runner
- `black` / `ruff` — code formatter / linter
- `mypy` — static type checker
- `python -m pdb` — debugger

### Exercises

- Build a CLI tool with `argparse` that reads a JSON file, filters records by criteria, and outputs results as CSV
- Implement a class hierarchy with inheritance, dunder methods, and a context manager
- Write a test suite with pytest that uses fixtures, parametrize, and mocking — achieve >90% coverage on your CLI tool
- Create a virtual environment, install dependencies, freeze them to requirements.txt, recreate the environment from scratch
- Write a decorator that logs function calls with timestamps and arguments
- Parse a complex log file: extract patterns with regex, aggregate statistics with Counter, output a summary report
- Write a script using `subprocess.run()` to automate a multi-step system administration task

### Proficiency Criteria

You write Pythonic code that passes `ruff` and `mypy` without warnings. You choose the right data structure for each problem. You test your code with `pytest` and know what to mock and what not to. You manage dependencies with virtual environments without thinking. You can read and understand any Python script you encounter in the wild.

---

## Section 8: Version Control

- **Position**: 8
- **Icon**: git-branch
- **Prerequisites**: Section 7

### Learning Objectives

1. You can explain Git's object model (blobs, trees, commits, tags) and how the DAG (directed acyclic graph) works.
2. You can create repositories, stage changes, commit with meaningful messages, and manage your working tree.
3. You can create, switch, merge, and delete branches, and resolve merge conflicts.
4. You can rebase, cherry-pick, and use interactive rebase to clean up commit history.
5. You can use Git's reflog to recover from mistakes (lost commits, bad rebases, accidental resets).
6. You can work with remotes: clone, fetch, pull, push, and manage upstream tracking.
7. You can use GitHub (or GitLab, Bitbucket) for pull requests, code reviews, issues, and collaboration workflows.
8. You can write `.gitignore` files and manage what gets tracked.
9. You can use `git bisect` to find the commit that introduced a bug.
10. You can use `git stash` to manage work-in-progress changes.
11. You can configure Git hooks for pre-commit checks and automation.
12. You can explain branching strategies (Git Flow, GitHub Flow, trunk-based) and their tradeoffs.

### Topic Outline

1. **Version control concepts**
   - What version control solves: history, collaboration, branching, rollback
   - Centralized (SVN) vs. distributed (Git, Mercurial): why distributed won
   - Git's design principles: speed, data integrity (SHA-1 hashing), distributed, branching is cheap
2. **Git's object model**
   - Blob: file content (not filename), content-addressable by SHA-1 hash
   - Tree: directory listing, maps names to blobs and other trees
   - Commit: snapshot pointer (tree), parent(s), author, committer, message
   - Tag: named pointer to a commit, annotated vs. lightweight
   - The DAG: commits form a directed acyclic graph, branches are pointers into the graph
   - `git cat-file`, `git ls-tree`, `git rev-parse`: inspecting objects directly
   - Pack files: how Git compresses objects for storage and transfer
3. **Basic workflow**
   - `git init`: create a repository
   - `git clone`: copy a remote repository
   - The three areas: working directory, staging area (index), repository (committed)
   - `git add`: stage changes (hunks with `-p`, specific files, all with `.`)
   - `git commit`: create a snapshot, `-m` for inline message, `--amend` for fixing last commit
   - `git status`: see what has changed
   - `git diff`: unstaged changes, `--staged` for staged changes, `HEAD~1` for last commit
   - `git log`: `--oneline`, `--graph`, `--all`, `--author`, `--since`, `--grep`
   - `git show`: inspect a specific commit
   - Commit messages: imperative mood, 50-char subject, blank line, body wrapping at 72 chars
4. **Branching and merging**
   - Branches: lightweight pointers to commits, `git branch`, `git switch` (or `git checkout`)
   - Creating branches: `git switch -c feature-branch`
   - Merging: `git merge`, fast-forward vs. three-way merge, merge commits
   - Merge conflicts: what causes them, how to read conflict markers (`<<<<`, `====`, `>>>>`), resolution workflow
   - `git mergetool`: configuring external merge tools (vimdiff, meld, VS Code)
   - Deleting branches: `git branch -d` (safe) vs. `-D` (force)
5. **Rewriting history**
   - `git rebase`: replay commits on a new base, linear history
   - Interactive rebase: `git rebase -i`, pick, squash, fixup, reword, edit, drop
   - `git cherry-pick`: apply specific commits from another branch
   - `git revert`: create a new commit that undoes a previous one (safe for shared history)
   - `git reset`: `--soft` (keep staged), `--mixed` (keep working, unstage), `--hard` (discard everything)
   - The golden rule: never rebase commits that have been pushed to a shared branch
   - `git commit --fixup` and `git rebase --autosquash`: clean fixup workflow
6. **Recovery with reflog**
   - `git reflog`: log of where HEAD has been, last resort for recovery
   - Recovering lost commits: find SHA in reflog, `git checkout` or `git cherry-pick`
   - Recovering from bad rebase: `git reset --hard ORIG_HEAD` or reflog
   - Dangling objects: `git fsck --lost-found`
   - `git gc` and object pruning: how Git cleans up, when dangling objects expire
7. **Working with remotes**
   - Remote: a bookmark to another repository, `origin` convention
   - `git remote add`, `git remote -v`, `git remote rename`, `git remote remove`
   - `git fetch`: download objects and refs without merging
   - `git pull`: fetch + merge (or `--rebase` for fetch + rebase)
   - `git push`: upload commits, `-u` to set upstream, `--force-with-lease` (safer than `--force`)
   - Tracking branches: `origin/main`, `git branch -vv`
   - Forks: personal copies, upstream remotes, keeping forks in sync
8. **Collaboration workflows**
   - Pull requests (GitHub) / Merge requests (GitLab): propose, review, merge
   - Code review: commenting on diffs, requesting changes, approving
   - Issues: bug reports, feature requests, linking PRs to issues
   - Branch protection: required reviews, status checks, no force push
   - CODEOWNERS: automatic review assignment
   - Branching strategies:
     - GitHub Flow: main + feature branches, deploy from main
     - Git Flow: main, develop, feature, release, hotfix branches
     - Trunk-based development: short-lived branches, frequent merges to main
     - Choosing a strategy: team size, release cadence, risk tolerance
9. **Advanced Git**
   - `.gitignore`: patterns, negation (`!`), global gitignore, `.git/info/exclude`
   - `git stash`: `stash`, `stash pop`, `stash list`, `stash apply`, `stash drop`, named stashes
   - `git bisect`: binary search for the commit that introduced a bug, automated with `git bisect run`
   - `git blame`: line-by-line attribution, `-w` (ignore whitespace), `-C` (detect moved code)
   - `git worktree`: multiple working trees for one repository
   - Git hooks: `.git/hooks/`, pre-commit, commit-msg, pre-push, post-merge, client vs. server hooks
   - Pre-commit framework: `.pre-commit-config.yaml`, shared hook configurations
   - `git submodule` vs. `git subtree`: including other repos, tradeoffs
   - Signing commits: GPG/SSH signatures, `git commit -S`, verification
   - Large files: `git-lfs`, `.gitattributes`, when to use it

### Key Concepts

- Git stores snapshots, not diffs — every commit is a complete tree of content
- Branches are cheap pointers — create, merge, and delete them freely
- The reflog is your safety net — you can almost always recover from mistakes
- Rebase for clean history, merge for preserving context — know when to use each
- Never rewrite history that others have already pulled

### Essential Commands and Tools

- `git init` / `clone` / `add` / `commit` / `push` / `pull` — daily workflow
- `git branch` / `switch` / `merge` / `rebase` — branching
- `git log` / `diff` / `blame` / `bisect` — investigation
- `git stash` / `reflog` / `reset` / `revert` — recovery
- `git remote` / `fetch` — remote management
- `gh` — GitHub CLI for PRs, issues, repos

### Exercises

- Initialize a repo, make a series of commits, create a branch, make conflicting changes, merge and resolve conflicts
- Use interactive rebase to squash, reorder, and reword commits into a clean history
- Simulate a lost commit: reset --hard, then recover using reflog
- Use `git bisect` (with `git bisect run` and a test script) to find a bug-introducing commit in a 100+ commit history
- Set up a pre-commit hook that runs `shellcheck` on `.sh` files and blocks commits with warnings
- Fork a repository, create a feature branch, submit a pull request, respond to review feedback, merge
- Compare Git Flow vs. GitHub Flow: create a release using each strategy and document the tradeoffs

### Proficiency Criteria

You use Git as a thinking tool, not just a save button. You commit early and often with meaningful messages. You rebase feature branches before merging. You recover from mistakes using reflog without panic. You can explain Git's object model to someone who has never used version control. You choose between merge and rebase with informed reasoning.

---

## Section 9: Networking Fundamentals

- **Position**: 9
- **Icon**: network
- **Prerequisites**: Section 4

### Learning Objectives

1. You can explain the OSI and TCP/IP models and map real protocols to each layer.
2. You can describe how IP addressing works: IPv4, IPv6, subnetting, CIDR, private vs. public ranges.
3. You can explain TCP (three-way handshake, flow control, congestion control) and UDP and when to use each.
4. You can describe DNS resolution end-to-end: recursive resolvers, root servers, TLD servers, authoritative servers, caching, record types.
5. You can explain HTTP/1.1, HTTP/2, and HTTP/3, including methods, headers, status codes, and connection management.
6. You can configure and troubleshoot firewalls, NAT, and port forwarding.
7. You can use packet capture tools (tcpdump, Wireshark) to analyze network traffic at the packet level.
8. You can configure and troubleshoot SSH: key-based authentication, config files, tunneling, agent forwarding.
9. You can explain routing: static routes, default gateways, routing tables, and the basics of dynamic routing (OSPF, BGP).
10. You can describe network overlays: VLAN, VXLAN, and why they matter for virtualization and containers.
11. You can explain TLS: certificate chain, handshake, cipher suites, mTLS.
12. You can troubleshoot connectivity issues systematically using the right tool at each layer.

### Topic Outline

1. **Network models**
   - OSI model: physical, data link, network, transport, session, presentation, application — what each layer does
   - TCP/IP model: network interface, internet, transport, application — the practical model
   - Why models matter: troubleshooting by layer, understanding where protocols operate
   - Encapsulation: data → segment → packet → frame — headers at each layer
   - PDUs: frame (L2), packet (L3), segment/datagram (L4)
2. **Physical and data link layer**
   - Ethernet: IEEE 802.3, frames, MAC addresses (48-bit, OUI), broadcast domain
   - Switches: MAC address table, flooding, learning, forwarding
   - VLANs: 802.1Q tagging, trunk ports, access ports, VLAN isolation
   - ARP: address resolution, ARP table, ARP spoofing
   - Wi-Fi: 802.11 standards, channels, frequencies (2.4GHz vs. 5GHz vs. 6GHz)
   - MTU: maximum transmission unit, jumbo frames, fragmentation, path MTU discovery
3. **IP addressing and subnetting**
   - IPv4: 32-bit addresses, dotted-decimal notation, address classes (historical)
   - CIDR: classless inter-domain routing, `/24`, `/16`, `/8` — variable-length subnet masks
   - Subnetting: network address, broadcast address, host range, subnet calculation
   - Private ranges: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, RFC 1918
   - Special addresses: 127.0.0.0/8 (loopback), 169.254.0.0/16 (link-local), 0.0.0.0, 255.255.255.255
   - DHCP: discover, offer, request, acknowledge — how devices get IP addresses
   - IPv6: 128-bit addresses, notation, `::` abbreviation, link-local (`fe80::`), global unicast, SLAAC, DHCPv6
   - IPv4 vs. IPv6: dual-stack, transition mechanisms, why IPv6 adoption is slow
4. **Transport layer**
   - TCP: connection-oriented, reliable, ordered
     - Three-way handshake: SYN → SYN-ACK → ACK
     - Sequence numbers and acknowledgments
     - Flow control: sliding window, window size, window scaling
     - Congestion control: slow start, congestion avoidance, fast retransmit, fast recovery, TCP algorithms (Reno, Cubic, BBR)
     - Connection teardown: FIN → ACK → FIN → ACK, TIME_WAIT state
     - TCP options: MSS, timestamps, selective acknowledgment (SACK)
   - UDP: connectionless, unreliable, unordered, low overhead
     - Use cases: DNS, DHCP, VoIP, gaming, streaming, QUIC
   - Ports: well-known (0-1023), registered (1024-49151), ephemeral (49152-65535)
   - Common ports: 22 (SSH), 53 (DNS), 80 (HTTP), 443 (HTTPS), 25/587 (SMTP), 3306 (MySQL), 5432 (PostgreSQL)
   - Socket: IP + port combination, `AF_INET`/`AF_INET6`, `SOCK_STREAM`/`SOCK_DGRAM`
5. **DNS**
   - What DNS does: name → IP resolution (and reverse)
   - Hierarchy: root (`.`) → TLD (`.com`, `.org`, `.io`) → second-level → subdomains
   - Resolution process: stub resolver → recursive resolver → root → TLD → authoritative
   - Record types: A (IPv4), AAAA (IPv6), CNAME (alias), MX (mail), NS (nameserver), TXT (text), SRV (service), PTR (reverse), SOA (authority), CAA (certificate authority)
   - TTL: time to live, caching, propagation delays
   - Zones and zone files: SOA records, zone transfers (AXFR/IXFR)
   - DNS security: DNSSEC (signing zones, chain of trust), DoH (DNS over HTTPS), DoT (DNS over TLS)
   - Common DNS tools: `dig`, `nslookup`, `host`, `drill`
   - Split-horizon DNS: different answers for internal vs. external queries
6. **HTTP**
   - HTTP/1.1: text-based, request/response, persistent connections, pipelining
   - Methods: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS — semantics and idempotency
   - Headers: `Host`, `Content-Type`, `Authorization`, `Accept`, `Cache-Control`, `User-Agent`, `Cookie`/`Set-Cookie`
   - Status codes: 1xx (informational), 2xx (success), 3xx (redirect), 4xx (client error), 5xx (server error)
   - Common codes: 200, 201, 204, 301, 302, 304, 400, 401, 403, 404, 405, 429, 500, 502, 503, 504
   - HTTP/2: binary framing, multiplexing, header compression (HPACK), server push
   - HTTP/3: QUIC (UDP-based), 0-RTT handshake, improved multiplexing (no head-of-line blocking)
   - Cookies: setting, reading, `Secure`, `HttpOnly`, `SameSite`, session management
   - Caching: `Cache-Control`, `ETag`, `If-None-Match`, `If-Modified-Since`, CDN caching
7. **Routing**
   - Routing table: destination, gateway, interface, metric
   - Default gateway: the route of last resort
   - Static routes: manually configured, `ip route add`
   - Dynamic routing protocols:
     - OSPF: link-state, areas, SPF algorithm, internal routing
     - BGP: path-vector, AS numbers, eBGP vs. iBGP, the protocol that runs the internet
     - RIP: distance-vector, hop count, mostly historical
   - NAT: network address translation, SNAT, DNAT, PAT (port address translation), NAT traversal challenges
   - Port forwarding: expose internal services, `iptables` DNAT rules
   - Traceroute: how it works (TTL expiry), interpreting output, `mtr` for continuous monitoring
8. **Network overlays**
   - Why overlays: limited VLAN IDs (4096), multi-tenant isolation, VM/container mobility
   - VXLAN: encapsulation in UDP, 24-bit VNI (16 million networks), VTEP
   - GRE: generic routing encapsulation, point-to-point tunnels
   - IPsec: site-to-site and remote access VPNs, tunnel mode vs. transport mode, IKE
   - WireGuard: modern VPN, simple configuration, kernel-level performance
   - Software-defined networking: control plane vs. data plane separation (conceptual overview)
9. **TLS and encryption in transit**
   - TLS handshake: ClientHello → ServerHello → certificate → key exchange → encrypted
   - Certificate chain: root CA → intermediate CA → leaf certificate
   - Certificate validation: hostname match, expiry, chain of trust, revocation (CRL, OCSP)
   - Cipher suites: key exchange (ECDHE), authentication (RSA, ECDSA), encryption (AES-GCM), hash (SHA-256)
   - TLS versions: 1.2 (minimum acceptable), 1.3 (preferred, faster handshake)
   - mTLS: mutual TLS, both sides present certificates, service-to-service authentication
   - `openssl` commands: generating keys, CSRs, self-signed certificates, inspecting certificates, testing connections
   - Let's Encrypt and ACME protocol: automated certificate issuance
10. **SSH**
    - How SSH works: key exchange, authentication, encrypted channel
    - Password vs. key-based authentication: why keys are preferred
    - Key generation: `ssh-keygen`, key types (Ed25519 preferred, RSA), passphrase protection
    - `~/.ssh/config`: host aliases, default users, identity files, ProxyJump
    - `ssh-agent`: key caching, `ssh-add`, agent forwarding (`-A`) and its security implications
    - Port forwarding: local (`-L`), remote (`-R`), dynamic/SOCKS (`-D`)
    - `scp` and `sftp`: file transfer over SSH, `rsync` over SSH
    - SSH hardening: disable password auth, disable root login, change port, `fail2ban`
    - Jump hosts / bastion hosts: `ProxyJump`, `ProxyCommand`
11. **Firewalls and network security**
    - Stateful vs. stateless firewalls: connection tracking, return traffic
    - `iptables`: tables (filter, nat, mangle), chains (INPUT, OUTPUT, FORWARD), rules, targets (ACCEPT, DROP, REJECT)
    - `nftables`: the successor to iptables, syntax differences, named tables and chains
    - `ufw` (Ubuntu), `firewalld` (RHEL/Fedora): frontend tools, zones, services
    - Cloud security groups: AWS Security Groups, Azure NSGs, GCP Firewall Rules — concept mapping
    - Network ACLs: subnet-level filtering (stateless in AWS, varies by provider)
12. **Troubleshooting methodology**
    - Layer-by-layer approach: start at L1 (cable/link), work up to L7 (application)
    - Tools by layer:
      - L1/L2: `ip link`, `ethtool`, cable check
      - L3: `ping`, `ip addr`, `ip route`, `traceroute`/`mtr`
      - L4: `ss`, `nc` (netcat), `telnet`
      - L7: `curl`, `wget`, `dig`, application logs
    - Packet capture: `tcpdump` (CLI), Wireshark (GUI), capture filters vs. display filters, common filters
    - `tcpdump` essentials: `-i` (interface), `-n` (no DNS), `-w` (write pcap), `-r` (read pcap), BPF filter syntax
    - Common issues: DNS failure, routing loops, MTU mismatch, firewall blocking, certificate errors, port conflicts

### Key Concepts

- The OSI/TCP model gives you a mental framework for troubleshooting — always think in layers
- TCP guarantees delivery and ordering at the cost of latency; UDP trades reliability for speed
- DNS is a distributed hierarchical database — understanding it end-to-end eliminates an entire class of "it's a DNS problem" mysteries
- NAT hides private IPs behind public ones — it is not a security mechanism, it is an addressing workaround
- TLS provides confidentiality, integrity, and authentication — learn the handshake

### Essential Commands and Tools

- `ip addr` / `ip route` / `ip link` — interface and routing management
- `ss` — socket statistics
- `ping` / `traceroute` / `mtr` — connectivity testing
- `dig` / `nslookup` — DNS queries
- `curl` — HTTP requests
- `tcpdump` — packet capture
- `ssh` / `ssh-keygen` / `ssh-agent` — secure shell
- `openssl` — TLS/certificate operations
- `iptables` / `nftables` / `ufw` — firewall management
- `nc` (netcat) — TCP/UDP connectivity testing

### Exercises

- Subnet a /24 into four /26 networks: calculate network addresses, broadcast addresses, and usable host ranges by hand
- Capture HTTP traffic with `tcpdump`, save to pcap, open in Wireshark, follow a TCP stream, identify the HTTP request and response
- Set up SSH key-based authentication, configure `~/.ssh/config` with aliases, create a local port forward to access a remote service
- Query DNS for a domain: trace the full resolution path from root to authoritative using `dig +trace`
- Configure `iptables` rules to allow SSH and HTTP, block everything else, verify with `nc`
- Generate a self-signed TLS certificate with `openssl`, configure a web server to use it, inspect the certificate chain
- Troubleshoot a broken network connection using the layer-by-layer methodology: identify which layer is failing and fix it
- Set up a VLAN on a Linux machine using `ip link` and verify isolation

### Proficiency Criteria

You can troubleshoot any network issue by working through the layers systematically. You can read `tcpdump` output and identify what is happening at the packet level. You can explain the TCP handshake, DNS resolution, and TLS handshake from memory. You can subnet in your head. You understand NAT, routing, and firewalls well enough to design a small network.

---

## Section 10: API Fundamentals

- **Position**: 10
- **Icon**: plug
- **Prerequisites**: Sections 7, 9

### Learning Objectives

1. You can explain what an API is, why APIs exist, and the difference between internal, partner, and public APIs.
2. You can describe REST architectural constraints and evaluate whether an API is truly RESTful.
3. You can make API calls using curl and Python's `requests` library, handling headers, query parameters, and request bodies.
4. You can authenticate with APIs using API keys, Basic auth, Bearer tokens, and OAuth 2.0 flows.
5. You can read and interpret JSON and YAML fluently, including nested structures and arrays.
6. You can parse and generate JSON/YAML programmatically in both shell (jq) and Python.
7. You can explain API versioning strategies, pagination patterns, and rate limiting.
8. You can describe GraphQL and gRPC as alternatives to REST and explain when each is appropriate.
9. You can read API documentation (OpenAPI/Swagger) and use it to construct valid requests.
10. You can design a simple RESTful API with proper resource naming, HTTP methods, and status codes.
11. You can implement error handling, retries with exponential backoff, and idempotency in API clients.
12. You can explain webhooks and event-driven API patterns.

### Topic Outline

1. **What is an API?**
   - API defined: a contract between software components
   - Why APIs exist: decoupling, reuse, abstraction, integration
   - API types: library/SDK, OS system calls, web APIs (the focus of this section)
   - API consumers vs. producers: client-server relationship
   - Internal APIs (microservices), partner APIs (B2B), public APIs (open ecosystem)
2. **REST architecture**
   - REST constraints: client-server, stateless, cacheable, uniform interface, layered system, code on demand (optional)
   - Resources: nouns, not verbs — `/users`, `/orders/{id}`, `/users/{id}/orders`
   - HTTP methods mapped to CRUD: GET (read), POST (create), PUT (replace), PATCH (update), DELETE (remove)
   - Idempotency: GET, PUT, DELETE are idempotent; POST is not — why this matters
   - HATEOAS: hypermedia as the engine of application state — what it means, why few APIs implement it
   - Richardson Maturity Model: Level 0 (single URL) → Level 3 (HATEOAS)
   - REST vs. "REST-ish": most APIs are pragmatic, not purist
3. **Data formats**
   - JSON: syntax (objects, arrays, strings, numbers, booleans, null), nesting, common pitfalls
   - YAML: superset of JSON, indentation-based, anchors and aliases, multi-line strings, common pitfalls (Norway problem, implicit typing)
   - JSON vs. YAML: JSON for APIs and machine parsing, YAML for configuration and human editing
   - XML: legacy format, still present in SOAP and enterprise systems
   - Protocol Buffers (protobuf): binary serialization, schema definition, efficiency — used with gRPC
   - `jq`: JSON processing on the command line — `.key`, `.[0]`, `| select()`, `| map()`, `@csv`, raw output (`-r`)
   - `yq`: YAML processing, compatible syntax with jq
4. **Making API calls**
   - `curl` deep dive:
     - Methods: `-X GET`, `-X POST`, `-X PUT`, `-X DELETE`
     - Headers: `-H "Content-Type: application/json"`, `-H "Authorization: Bearer TOKEN"`
     - Data: `-d '{"key":"value"}'`, `--data-binary @file.json`
     - Query parameters: URL-encoded in the URL
     - Response inspection: `-v` (verbose), `-i` (include headers), `-o` (output file), `-w` (format output)
     - Following redirects: `-L`
     - Silent mode: `-s`, `-S` (show errors)
   - Python `requests`:
     - `requests.get()`, `.post()`, `.put()`, `.patch()`, `.delete()`
     - `params={}` for query strings, `json={}` for JSON bodies, `headers={}`
     - `response.status_code`, `.json()`, `.text`, `.headers`, `.raise_for_status()`
     - Sessions: `requests.Session()` for persistent connections and shared configuration
     - Timeouts: always set `timeout=` to avoid hanging requests
   - `httpie`: human-friendly alternative to curl
5. **Authentication and authorization**
   - API keys: header-based (`X-API-Key`), query parameter — simple but limited
   - Basic auth: Base64-encoded `username:password`, must use HTTPS
   - Bearer tokens: `Authorization: Bearer <token>`, typically JWT
   - JWT (JSON Web Tokens): header.payload.signature, claims, expiration, verification, no encryption (signed only)
   - OAuth 2.0: authorization framework, not authentication
     - Roles: resource owner, client, authorization server, resource server
     - Flows: authorization code (web apps), client credentials (service-to-service), device code (CLI/IoT)
     - Scopes: granular permission sets
     - Tokens: access token (short-lived), refresh token (long-lived)
   - API key vs. OAuth: when to use each
   - Service accounts: non-human identities for automation
6. **API design patterns**
   - Versioning: URL path (`/v1/`), header (`Accept: application/vnd.api+json;version=1`), query parameter (`?version=1`) — tradeoffs
   - Pagination: offset-based (`?page=2&per_page=20`), cursor-based (`?cursor=abc123`), keyset — when to use each
   - Filtering and sorting: query parameters (`?status=active&sort=-created_at`)
   - Rate limiting: `429 Too Many Requests`, `X-RateLimit-*` headers, backoff strategies
   - Error responses: consistent format (`{"error": {"code": "...", "message": "..."}}`)
   - Bulk operations: batch endpoints, async processing with status polling
   - Idempotency keys: client-generated unique IDs for safe retries
7. **Client resilience patterns**
   - Retries: when to retry (5xx, timeouts, network errors), when not to retry (4xx)
   - Exponential backoff: base delay × 2^attempt, jitter, max retries
   - Circuit breaker: detect failures, stop sending requests, test recovery
   - Timeouts: connect timeout vs. read timeout, why defaults are dangerous
   - Idempotent requests: safe to retry PUT/DELETE, use idempotency keys for POST
8. **Beyond REST**
   - GraphQL: query language for APIs, client specifies shape of response, single endpoint, schema, resolvers
     - Advantages: no over-fetching, no under-fetching, strong typing
     - Disadvantages: caching complexity, N+1 queries, learning curve
   - gRPC: high-performance RPC framework, Protocol Buffers, HTTP/2, streaming
     - Advantages: performance, strong typing, bi-directional streaming
     - Disadvantages: not browser-native, binary format harder to debug
   - WebSockets: full-duplex communication, persistent connection, real-time use cases
   - Webhooks: server pushes events to client URL, callback registration, signature verification
   - Server-Sent Events (SSE): one-way server → client streaming over HTTP
   - When to use what: REST for CRUD, GraphQL for complex reads, gRPC for internal services, WebSockets for real-time
9. **API documentation and specification**
   - OpenAPI (Swagger): YAML/JSON specification, paths, operations, schemas, parameters, responses
   - Reading OpenAPI specs: understanding endpoints, request/response shapes, authentication requirements
   - Swagger UI: interactive API explorer
   - Postman / Insomnia: GUI tools for testing APIs, collections, environments
   - API-first design: write the spec before the code

### Key Concepts

- An API is a contract — both sides must agree on the format, and changes must be managed
- REST is an architectural style with constraints, not just "JSON over HTTP"
- Always set timeouts and implement retries with backoff for API clients
- Authentication (who you are) and authorization (what you can do) are separate concerns
- Choose the right API paradigm: REST for simplicity, GraphQL for flexible queries, gRPC for performance

### Essential Commands and Tools

- `curl` — HTTP client
- `jq` — JSON processor
- `yq` — YAML processor
- `python3 -c "import requests"` — Python HTTP library
- `httpie` — human-friendly HTTP client
- `openssl` — TLS debugging for API connections

### Exercises

- Use `curl` to interact with a public REST API: list resources, create, update, and delete, handling authentication headers
- Write a Python script that paginates through an API, handles rate limiting (429), and implements exponential backoff
- Parse a complex nested JSON response with `jq`: extract specific fields, filter arrays, transform into CSV
- Read an OpenAPI spec and construct valid requests for every endpoint without additional documentation
- Implement OAuth 2.0 client credentials flow in Python: request a token, use it for authenticated API calls, handle token refresh
- Design a RESTful API for a simple application: define resources, endpoints, methods, request/response schemas, error responses, and versioning strategy
- Set up a webhook receiver (simple HTTP server) and verify incoming webhook signatures

### Proficiency Criteria

You can interact with any REST API given its documentation. You write resilient API clients with timeouts, retries, and error handling. You can authenticate using any common method. You can parse and transform JSON and YAML on the command line and in Python without hesitation. You understand when REST is not the right choice and can explain the alternatives.

---

## Section 11: Security Fundamentals

- **Position**: 11
- **Icon**: shield
- **Prerequisites**: Section 9

### Learning Objectives

1. You can explain the CIA triad (confidentiality, integrity, availability) and apply it to real-world scenarios.
2. You can describe symmetric vs. asymmetric encryption, hashing, and digital signatures — and when to use each.
3. You can explain PKI: certificate authorities, certificate chains, X.509 certificates, and certificate lifecycle management.
4. You can describe authentication factors (knowledge, possession, inherence) and implement MFA concepts.
5. You can explain authorization models: RBAC, ABAC, ACLs, and least privilege.
6. You can manage secrets securely: environment variables, secret managers, rotation, and why hardcoded secrets are a critical vulnerability.
7. You can identify and explain OWASP Top 10 vulnerabilities and their mitigations.
8. You can describe defense in depth and zero trust architecture principles.
9. You can perform basic security hardening on a Linux system.
10. You can explain supply chain security: dependency scanning, SBOM, artifact signing.
11. You can describe incident response basics: detection, containment, eradication, recovery, lessons learned.
12. You can explain compliance frameworks at a high level: SOC 2, ISO 27001, PCI DSS, HIPAA — what they require and why.

### Topic Outline

1. **Security principles**
   - CIA triad: confidentiality (prevent unauthorized access), integrity (prevent unauthorized modification), availability (ensure access when needed)
   - AAA: authentication, authorization, accounting
   - Least privilege: minimum permissions required, blast radius reduction
   - Defense in depth: multiple layers of security controls
   - Zero trust: never trust, always verify — identity-based access, micro-segmentation
   - Threat modeling: identifying assets, threats, vulnerabilities, attack vectors — STRIDE framework
   - Risk assessment: likelihood × impact, risk acceptance vs. mitigation vs. transfer
2. **Cryptography**
   - Symmetric encryption: same key for encrypt/decrypt, AES (AES-128, AES-256), ChaCha20
     - Modes: ECB (insecure), CBC, GCM (authenticated encryption) — why mode matters
     - Use cases: encrypting data at rest, disk encryption, TLS record encryption
   - Asymmetric encryption: public/private key pairs, RSA, ECDSA, Ed25519
     - Use cases: key exchange, digital signatures, TLS handshake, SSH keys
     - Performance: slower than symmetric — used to exchange symmetric keys
   - Hashing: one-way functions, SHA-256, SHA-3, BLAKE2
     - Properties: deterministic, fixed-size output, avalanche effect, collision resistance
     - Use cases: password storage (with salt), file integrity verification, digital signatures
     - Password hashing: bcrypt, scrypt, Argon2 — why general-purpose hashes (SHA-256) are wrong for passwords
   - HMACs: hash-based message authentication codes, ensuring integrity + authenticity
   - Digital signatures: hash + asymmetric encryption, non-repudiation
   - Key exchange: Diffie-Hellman, ECDHE — how two parties agree on a shared secret
   - Randomness: `/dev/urandom` vs. `/dev/random`, CSPRNG, why randomness is critical
3. **PKI and certificates**
   - Certificate authorities (CAs): root CAs, intermediate CAs, trust stores
   - X.509 certificates: subject, issuer, serial number, validity period, public key, extensions, SAN (Subject Alternative Name)
   - Certificate chain validation: leaf → intermediate → root, browser/OS trust stores
   - Certificate lifecycle: generation, signing (CSR), distribution, renewal, revocation
   - Revocation: CRL (Certificate Revocation List), OCSP (Online Certificate Status Protocol), OCSP stapling
   - Let's Encrypt: ACME protocol, automated issuance, 90-day certificates
   - Self-signed certificates: when appropriate (development, internal), when not (production, public-facing)
   - Certificate pinning: what it is, risks, alternatives
   - `openssl` operations: generate key, create CSR, sign certificate, inspect certificate, verify chain
4. **Authentication**
   - Authentication factors: knowledge (password), possession (token, phone), inherence (biometric)
   - Multi-factor authentication (MFA): combining factors, TOTP (time-based one-time password), FIDO2/WebAuthn, push notifications
   - Password security: complexity requirements, length over complexity, password managers, credential stuffing
   - SSO (Single Sign-On): SAML, OIDC (OpenID Connect), federation
   - OAuth 2.0 vs. OIDC: authorization vs. authentication, ID tokens
   - Service authentication: API keys, client certificates (mTLS), service accounts, workload identity
   - SSH authentication: keys vs. passwords, certificate-based SSH
5. **Authorization**
   - RBAC (Role-Based Access Control): roles, permissions, role hierarchy
   - ABAC (Attribute-Based Access Control): policies based on attributes (user, resource, environment)
   - ACLs: per-resource permission lists
   - Policy engines: OPA (Open Policy Agent), Cedar — declarative policy as code
   - Least privilege implementation: scoped tokens, temporary credentials, just-in-time access
   - Cloud IAM concepts: principals, policies, roles, permissions — how AWS IAM, Azure RBAC, GCP IAM implement these
6. **Secrets management**
   - What counts as a secret: passwords, API keys, tokens, certificates, database credentials, encryption keys
   - Why hardcoded secrets are critical vulnerabilities: source code exposure, version control history
   - Environment variables: better than hardcoded, still has risks (process inspection, logging, child process inheritance)
   - Secret managers: HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager — concept and workflow
   - Secret rotation: why rotate, automated rotation, zero-downtime rotation patterns
   - `.env` files: development convenience, never commit, `.gitignore` enforcement
   - Secret scanning: git-secrets, truffleHog, GitHub secret scanning — detecting leaked credentials
   - Sealing and unsealing: Vault's seal mechanism, Shamir's secret sharing (concept)
7. **Common vulnerabilities (OWASP Top 10)**
   - Injection: SQL injection, command injection, LDAP injection — parameterized queries as the fix
   - Broken authentication: weak passwords, credential stuffing, session fixation
   - Sensitive data exposure: unencrypted data at rest or in transit, missing TLS
   - XXE (XML External Entities): XML parser exploits
   - Broken access control: IDOR (Insecure Direct Object Reference), privilege escalation
   - Security misconfiguration: default credentials, unnecessary features enabled, missing patches
   - XSS (Cross-Site Scripting): reflected, stored, DOM-based — output encoding and CSP
   - Insecure deserialization: untrusted data deserialized into objects
   - Vulnerable dependencies: known CVEs in libraries, dependency scanning
   - Insufficient logging and monitoring: inability to detect breaches
   - SSRF (Server-Side Request Forgery): making servers request internal resources
8. **System hardening**
   - Linux hardening checklist:
     - Disable root SSH login, use key-based auth only
     - Configure firewall (allow-list approach)
     - Remove unnecessary packages and services
     - Enable automatic security updates (unattended-upgrades, dnf-automatic)
     - Set proper file permissions, remove world-writable files
     - Configure audit logging (`auditd`)
     - Enable SELinux/AppArmor in enforcing mode
     - Restrict kernel parameters (`sysctl` hardening)
   - CIS Benchmarks: industry-standard hardening guides, automated scanning
   - Patch management: staying current, vulnerability scanners (Nessus, OpenVAS, Trivy)
9. **Supply chain security**
   - Software supply chain: source code → build → dependencies → artifacts → deployment
   - Dependency scanning: `npm audit`, `pip-audit`, Dependabot, Snyk, Trivy
   - SBOM (Software Bill of Materials): CycloneDX, SPDX — what goes into your software
   - Artifact signing: Sigstore, cosign, Notary — verifying artifact integrity
   - Reproducible builds: same source → same binary, build provenance
   - SLSA framework: Supply-chain Levels for Software Artifacts, levels 1-4
10. **Incident response**
    - Phases: preparation → detection → containment → eradication → recovery → lessons learned
    - Detection: monitoring, alerting, anomaly detection, SIEM (Security Information and Event Management)
    - Containment: isolate affected systems, prevent lateral movement
    - Eradication: remove root cause, patch vulnerabilities
    - Recovery: restore from backups, verify integrity, gradual return to production
    - Post-incident: blameless post-mortems, timeline reconstruction, action items
    - Communication: stakeholder notification, regulatory requirements (breach notification laws)
11. **Compliance and frameworks**
    - SOC 2: Type I (design) vs. Type II (operating effectiveness), trust service criteria
    - ISO 27001: information security management system (ISMS), risk-based approach
    - PCI DSS: payment card data protection, 12 requirements
    - HIPAA: healthcare data protection, PHI (Protected Health Information)
    - GDPR: data protection, data subject rights, DPO requirements
    - What these frameworks share: policies, access control, encryption, logging, incident response, regular audits

### Key Concepts

- Security is layers, not a single solution — defense in depth
- Encrypt data at rest and in transit — no exceptions
- Secrets belong in secret managers, not in code, not in environment variables for production
- The attacker only needs one vulnerability; the defender must cover all of them
- Zero trust: verify every request, regardless of network location

### Essential Commands and Tools

- `openssl` — cryptographic operations, certificate management
- `ssh-keygen` — key generation
- `gpg` — encryption, signing
- `fail2ban` — intrusion prevention
- `auditd` / `ausearch` — Linux audit framework
- `lynis` — security auditing tool
- `trivy` — vulnerability scanner
- `vault` — secret management (HashiCorp)

### Exercises

- Generate a self-signed CA, issue a leaf certificate, configure a service to use it, verify the chain with `openssl verify`
- Harden a fresh Linux installation following CIS Benchmark guidelines: document every change and why
- Set up HashiCorp Vault (dev mode), store secrets, retrieve them programmatically, configure automatic rotation
- Identify OWASP Top 10 vulnerabilities in a deliberately vulnerable application (DVWA, Juice Shop, or similar)
- Implement a Python script that encrypts a file with AES-256-GCM, using a key derived from a password with Argon2
- Configure `auditd` rules to log file access to a sensitive directory, verify with `ausearch`
- Set up secret scanning on a Git repository and verify it catches a deliberately committed test credential
- Write an incident response runbook for a hypothetical credential leak scenario

### Proficiency Criteria

You can explain the CIA triad and map any security control to the principle it enforces. You can describe the TLS handshake, PKI chain of trust, and certificate lifecycle from memory. You manage secrets through proper tooling, never hardcoded. You can identify common vulnerabilities in code and infrastructure. You can harden a Linux system and explain every change you make.

---

## Section 12: CI/CD

- **Position**: 12
- **Icon**: refresh-cw
- **Prerequisites**: Sections 8, 11

### Learning Objectives

1. You can explain CI (continuous integration) and CD (continuous delivery vs. continuous deployment) and why they matter.
2. You can design and implement a complete CI/CD pipeline: build, test, scan, package, deploy.
3. You can write GitHub Actions workflows with triggers, jobs, steps, matrix strategies, and reusable workflows.
4. You can manage secrets, environment variables, and environment-specific configuration in pipelines.
5. You can implement testing stages: unit, integration, end-to-end, and know what belongs in CI vs. manual.
6. You can implement artifact management: build artifacts, container images, versioning, and caching.
7. You can describe deployment strategies: rolling, blue-green, canary, and feature flags — and their tradeoffs.
8. You can implement quality gates: linting, formatting, security scanning, test coverage thresholds.
9. You can troubleshoot failed pipelines by reading logs, reproducing locally, and isolating failures.
10. You can explain GitOps: declarative infrastructure, Git as source of truth, reconciliation loops.
11. You can describe how other CI/CD platforms work (GitLab CI, Jenkins, CircleCI) and map concepts across them.
12. You can implement pipeline security: least-privilege runners, signed artifacts, OIDC authentication.

### Topic Outline

1. **CI/CD concepts**
   - Continuous Integration: merge frequently, build and test automatically, detect problems early
   - Continuous Delivery: every commit is releasable, deployment is a manual decision
   - Continuous Deployment: every commit that passes the pipeline deploys automatically
   - The feedback loop: faster feedback → smaller changes → fewer problems → faster feedback
   - Pipeline stages: source → build → test → scan → package → deploy → monitor
   - Trunk-based development and CI: why long-lived branches break CI
2. **GitHub Actions**
   - Workflow files: `.github/workflows/*.yml`, YAML syntax
   - Triggers: `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual), `release`, `workflow_call` (reusable)
   - Events and filters: branch filters, path filters, tag filters
   - Jobs: run on runners, dependency with `needs`, conditional execution with `if`
   - Steps: `uses` (actions) vs. `run` (shell commands), `name`, `id`, `env`, `with`
   - Runners: GitHub-hosted (Ubuntu, Windows, macOS), self-hosted runners, runner labels
   - Matrix strategy: testing across multiple versions/platforms, `matrix`, `include`, `exclude`
   - Expressions and contexts: `${{ github.event_name }}`, `${{ secrets.TOKEN }}`, `${{ steps.id.outputs.value }}`
   - Caching: `actions/cache`, cache keys, restore keys, dependency caching (npm, pip, Go modules)
   - Artifacts: `actions/upload-artifact`, `actions/download-artifact`, sharing data between jobs
   - Secrets: repository secrets, environment secrets, organization secrets, GITHUB_TOKEN permissions
   - Environments: protection rules, required reviewers, wait timers, deployment branches
   - Reusable workflows: `workflow_call`, inputs, secrets inheritance, composability
   - Composite actions: custom actions composed of steps, `action.yml`
   - Concurrency: `concurrency` groups, canceling in-progress runs
3. **Pipeline design patterns**
   - Build stage: compile, bundle, generate assets
   - Test stage: unit tests (fast, many), integration tests (slower, fewer), E2E tests (slowest, fewest) — the testing pyramid
   - Lint and format stage: catch style issues before review, fail the pipeline if code does not pass
   - Security scanning: SAST (static analysis), DAST (dynamic analysis), SCA (software composition analysis), secret scanning
   - Container image build: build in CI, tag with commit SHA and semantic version, push to registry
   - Artifact versioning: semantic versioning, commit SHA tags, immutable artifacts
   - Dependency caching: speed up builds by caching downloaded dependencies
   - Parallelism: run independent jobs concurrently, fan-out/fan-in patterns
4. **Deployment strategies**
   - Rolling deployment: replace instances gradually, no downtime, rollback by continuing forward or re-deploying
   - Blue-green deployment: two identical environments, switch traffic atomically, instant rollback
   - Canary deployment: route a percentage of traffic to new version, monitor, expand or rollback
   - Feature flags: deploy code without activating it, gradual rollout, A/B testing, kill switch
   - Database migrations in CI/CD: forward-only migrations, backward-compatible changes, expand-contract pattern
   - Rollback strategies: re-deploy previous version, feature flag disable, database rollback considerations
   - Smoke tests: quick validation after deployment, automated health checks
5. **Quality gates**
   - Linting: `ruff` (Python), `eslint` (JavaScript), `shellcheck` (Bash), `hadolint` (Dockerfile)
   - Formatting: `black` (Python), `prettier` (JS/TS), `gofmt` (Go) — enforce in CI
   - Test coverage: threshold enforcement, coverage reports, what coverage misses
   - Security scanning tools: `trivy` (containers, IaC), `semgrep` (code patterns), `gitleaks` (secrets), `checkov` (IaC)
   - License compliance: scanning dependencies for license compatibility
   - Performance testing: load tests, benchmarks, regression detection
   - Required status checks: branch protection rules, mandatory CI passing before merge
6. **GitOps**
   - Principles: declarative configuration, Git as single source of truth, automated reconciliation, closed-loop feedback
   - GitOps workflow: push change to Git → controller detects drift → reconciles desired state
   - Tools: Argo CD, Flux — Kubernetes GitOps operators
   - Push vs. pull deployment: CI pushes vs. GitOps controller pulls — security implications
   - Environment promotion: dev → staging → production via Git branches or directories
   - Drift detection: desired state vs. actual state, automatic correction
7. **Other CI/CD platforms**
   - GitLab CI: `.gitlab-ci.yml`, stages, jobs, runners, shared vs. specific runners, pipelines
   - Jenkins: Jenkinsfile, declarative vs. scripted pipeline, plugins, agents, Blue Ocean UI
   - CircleCI: `.circleci/config.yml`, orbs, workflows, executors
   - Concept mapping: jobs ↔ stages, runners ↔ agents, secrets ↔ credentials — the vocabulary differs but concepts transfer
   - Cloud-native CI/CD: AWS CodePipeline, Azure DevOps Pipelines, GCP Cloud Build — provider-specific options
   - Choosing a CI/CD platform: GitHub integration, self-hosted needs, ecosystem, cost
8. **Pipeline security**
   - Least-privilege runners: minimize permissions, use scoped tokens
   - OIDC authentication: exchange CI tokens for cloud provider credentials, no long-lived secrets
   - Signed artifacts: sign container images, verify signatures at deployment
   - Supply chain integrity: pin action versions by SHA, audit third-party actions
   - Pull request security: fork restrictions, approval requirements, `pull_request_target` risks
   - Secret hygiene: masking in logs, rotation, separate secrets per environment

### Key Concepts

- CI is about integrating code frequently and catching problems early — it is a practice, not just a tool
- The pipeline is the automated path from commit to production — every stage adds confidence
- Quality gates enforce standards automatically — no human has to remember to check
- GitOps makes infrastructure changes auditable, reversible, and declarative
- Pipeline security is as important as application security — compromised CI can compromise everything

### Essential Commands and Tools

- `gh` — GitHub CLI for workflow management
- `act` — run GitHub Actions locally
- `docker` / `podman` — container image building in CI
- `trivy` — vulnerability scanning
- `semgrep` — static analysis
- `gitleaks` — secret scanning

### Exercises

- Write a GitHub Actions workflow that builds a Python application, runs tests, checks linting, and reports coverage
- Implement a matrix strategy that tests across Python 3.10, 3.11, and 3.12 on Ubuntu and macOS
- Set up environment-based deployments with protection rules: require manual approval for production
- Build and push a container image in CI, tagged with both commit SHA and semantic version
- Implement caching for dependencies and verify build time improvement
- Write a reusable workflow that other repositories can call
- Set up OIDC authentication between GitHub Actions and a cloud provider (or simulate the flow)
- Compare the same pipeline implemented in GitHub Actions and GitLab CI: document the similarities and differences

### Proficiency Criteria

You can design a complete CI/CD pipeline from scratch that includes building, testing, scanning, packaging, and deploying. You understand the testing pyramid and put the right tests in the right stages. You can troubleshoot failed builds by reading logs and reproducing locally. You manage secrets properly in pipelines. You can explain GitOps to someone who has never heard of it.

---

## Section 13: Containers

- **Position**: 13
- **Icon**: container
- **Prerequisites**: Section 12

### Learning Objectives

1. You can explain what containers are, how they differ from VMs, and the kernel features (namespaces, cgroups, overlay filesystems) that make them work.
2. You can build efficient container images with Dockerfiles: multi-stage builds, layer optimization, security best practices.
3. You can manage container lifecycle: run, stop, remove, inspect, exec, logs, resource limits.
4. You can configure container networking: bridge, host, none, custom networks, port mapping, DNS resolution between containers.
5. You can manage persistent data with volumes and bind mounts, and explain the difference.
6. You can write Docker Compose files for multi-container applications with dependencies, networks, and volumes.
7. You can explain the OCI specification: image spec, runtime spec, distribution spec.
8. You can describe container runtimes: containerd, CRI-O, runc — and how they relate to each other and to Docker.
9. You can build and run rootless containers and explain why they matter for security.
10. You can use BuildKit features: cache mounts, secret mounts, SSH forwarding, multi-platform builds.
11. You can scan container images for vulnerabilities and explain base image selection strategy.
12. You can explain container registries: push, pull, tagging, digest-based references, registry authentication.

### Topic Outline

1. **Container fundamentals**
   - What a container is: isolated process(es) sharing the host kernel
   - Containers vs. VMs: no guest OS, shared kernel, faster startup, lower overhead, weaker isolation
   - Kernel features that enable containers:
     - Namespaces: PID, network, mount, UTS, IPC, user, cgroup — isolation
     - cgroups v2: CPU, memory, I/O, PIDs — resource limits
     - Overlay filesystems: OverlayFS, layered file systems, copy-on-write
     - seccomp: system call filtering
     - Capabilities: fine-grained root privileges
   - Container history: chroot → FreeBSD jails → Solaris zones → LXC → Docker → OCI
2. **OCI specification**
   - OCI Image Spec: image manifest, config, layers (tar + gzip/zstd), content-addressable storage
   - OCI Runtime Spec: what a runtime must do — create, start, stop, delete containers from OCI bundles
   - OCI Distribution Spec: how images are pushed/pulled from registries
   - Why standards matter: interoperability between runtimes, builders, and registries
3. **Container runtimes**
   - High-level runtimes (container engines): Docker Engine, Podman
   - Low-level runtimes: runc (reference OCI runtime), crun (C, faster), gVisor (runsc, sandboxed kernel), Kata Containers (micro-VMs)
   - containerd: industry-standard container runtime, used by Docker and Kubernetes
   - CRI-O: lightweight runtime designed for Kubernetes CRI
   - Architecture: Docker CLI → Docker daemon → containerd → runc → container
   - Podman: daemonless, rootless by default, Docker CLI-compatible, `podman-compose`
   - nerdctl: containerd CLI with Docker-compatible commands
4. **Building images**
   - Dockerfile instructions: `FROM`, `RUN`, `COPY`, `ADD`, `WORKDIR`, `ENV`, `ARG`, `EXPOSE`, `USER`, `ENTRYPOINT`, `CMD`, `LABEL`, `HEALTHCHECK`, `SHELL`
   - `ENTRYPOINT` vs. `CMD`: exec form vs. shell form, how they combine, override behavior
   - Layer model: each instruction creates a layer, layers are cached, order matters for cache efficiency
   - Cache optimization: copy dependency files first, install dependencies, then copy application code
   - Multi-stage builds: separate build and runtime stages, copy artifacts between stages, smaller final images
   - `.dockerignore`: exclude files from build context, reduce build time and image size
   - Base image selection:
     - Full OS images: Ubuntu, Fedora — large, good for debugging
     - Slim images: `python:3.12-slim`, `node:20-slim` — reduced size
     - Alpine: musl libc (not glibc), very small, compatibility issues
     - Distroless: Google's minimal images, no shell, no package manager
     - Scratch: empty base, for static binaries (Go, Rust)
   - BuildKit:
     - Cache mounts: `--mount=type=cache` for package manager caches, dramatically faster rebuilds
     - Secret mounts: `--mount=type=secret` for build-time secrets (never baked into layers)
     - SSH mounts: `--mount=type=ssh` for private repo access during build
     - Multi-platform builds: `docker buildx build --platform linux/amd64,linux/arm64`
     - Build arguments: `ARG`, `--build-arg`, scoping rules
   - Image security:
     - Run as non-root: `USER 1000:1000`
     - Minimize installed packages: no editors, debuggers, or unnecessary tools in production images
     - Pin base image versions: `FROM python:3.12.1-slim-bookworm`, not `FROM python:latest`
     - Scan images: `trivy image`, `docker scout`, `grype`
     - Sign images: cosign, Notary
5. **Running containers**
   - `docker run`: `-d` (detach), `-it` (interactive TTY), `--name`, `--rm` (auto-remove)
   - Resource limits: `--memory`, `--cpus`, `--pids-limit`
   - Environment: `-e VAR=value`, `--env-file`
   - Port mapping: `-p host:container`, `-P` (random port)
   - Container lifecycle: `create` → `start` → `running` → `stop` → `remove`, `pause`/`unpause`
   - `docker exec`: run commands in a running container, `-it` for interactive
   - `docker logs`: stdout/stderr output, `-f` (follow), `--tail`, `--since`
   - `docker inspect`: detailed JSON metadata, `--format` for specific fields
   - `docker cp`: copy files between host and container
   - `docker stats`: real-time resource usage
   - `docker top`: processes running inside a container
   - Health checks: `HEALTHCHECK` in Dockerfile, `--health-cmd`, `--health-interval`, container health status
6. **Networking**
   - Bridge network: default, containers communicate via bridge, port mapping for external access
   - Host network: `--network host`, share host network stack, no isolation
   - None network: `--network none`, no networking, fully isolated
   - Custom bridge networks: `docker network create`, automatic DNS resolution by container name
   - Container-to-container communication: same network = DNS by name, different networks = no direct communication
   - DNS: embedded DNS server in custom networks, service discovery by container name
   - Network drivers: bridge, host, overlay (Swarm), macvlan (direct L2), ipvlan
   - Published ports: `-p 8080:80` (specific), `-p 80` (random host port), binding to specific IPs (`127.0.0.1:8080:80`)
7. **Storage**
   - Union/overlay filesystem: read-only layers + writable layer, copy-on-write
   - Volumes: managed by container runtime, persist beyond container lifecycle, `docker volume create/ls/rm/inspect`
   - Bind mounts: map host directory into container, `-v /host/path:/container/path`
   - tmpfs mounts: RAM-backed, never written to disk, good for sensitive data
   - Volume drivers: local, NFS, cloud-specific — extending storage backends
   - Volumes vs. bind mounts: volumes are managed and portable, bind mounts are host-dependent
   - Data persistence patterns: database data, configuration, logs, shared data between containers
8. **Docker Compose**
   - What Compose solves: multi-container application definition and lifecycle
   - `compose.yaml` (or `docker-compose.yml`): services, networks, volumes, configs, secrets
   - Service definition: `image`, `build`, `ports`, `environment`, `volumes`, `depends_on`, `restart`, `healthcheck`
   - `depends_on` with conditions: `condition: service_healthy`, `condition: service_started`
   - Networking in Compose: default network, custom networks, service name as DNS
   - Environment variable handling: `.env` file, `environment:`, `env_file:`, variable substitution
   - Compose profiles: group services for different scenarios (dev, test, debug)
   - `docker compose up -d`, `down`, `logs`, `ps`, `exec`, `build`, `pull`
   - Compose watch: `develop.watch` for automatic rebuilds during development
   - Override files: `compose.override.yml`, extending base configurations
9. **Rootless containers**
   - Why rootless matters: defense in depth, reduced blast radius if container is compromised
   - Docker rootless mode: user-namespace remapping, limitations
   - Podman rootless: default mode, no daemon, leverages user namespaces
   - User namespace remapping: `userns-remap`, `/etc/subuid`, `/etc/subgid`
   - Limitations: some operations require root (binding low ports, certain mount types)
10. **Container registries**
    - What a registry does: stores and distributes container images
    - Public registries: Docker Hub, GitHub Container Registry (ghcr.io), Quay.io
    - Private registries: self-hosted (Harbor, distribution), cloud-managed (ECR, ACR, GCR/Artifact Registry)
    - Tags vs. digests: `image:latest` (mutable pointer) vs. `image@sha256:abc...` (immutable content address)
    - Image naming: `registry/namespace/repository:tag`
    - Authentication: `docker login`, credential helpers, OIDC-based auth
    - Image scanning in registries: scan on push, policy enforcement
    - Multi-arch manifests: manifest lists pointing to platform-specific images

### Key Concepts

- Containers are isolated processes, not lightweight VMs — they share the host kernel
- Images are immutable, layered snapshots — containers are running instances of images
- OCI standards mean you are not locked into Docker — containerd, Podman, and others are interchangeable
- Multi-stage builds separate build dependencies from runtime, producing smaller and more secure images
- Rootless containers reduce the security impact of container escapes

### Essential Commands and Tools

- `docker` / `podman` — container management
- `docker compose` — multi-container orchestration
- `docker buildx` — advanced image building
- `trivy` — image vulnerability scanning
- `dive` — analyze image layers and size
- `cosign` — sign and verify container images
- `skopeo` — copy images between registries without pulling

### Exercises

- Build a multi-stage Dockerfile for a Python application: build dependencies in the first stage, copy only the application and runtime into a slim final stage
- Optimize a Dockerfile for layer caching: reorder instructions, verify cache hits with `docker build` output
- Write a `compose.yaml` for a three-service application (web, API, database) with proper networking, volumes, health checks, and dependency ordering
- Run the same container with Docker and Podman, compare behavior and commands
- Set up rootless Podman, run a container, verify it runs as a non-root user on the host
- Scan a container image with `trivy`, identify vulnerabilities, fix them by updating the base image or pinning package versions
- Use `dive` to analyze image layers, identify wasted space, reduce image size by 50%+
- Build a multi-platform image (amd64 + arm64) with `docker buildx` and push to a registry

### Proficiency Criteria

You can explain how containers work at the kernel level (namespaces, cgroups, overlayfs). You build optimized, secure images with multi-stage builds, non-root users, and pinned base images. You can design multi-container applications with Compose. You understand the OCI ecosystem beyond Docker. You can troubleshoot container networking, storage, and runtime issues.

---

## Section 14: Container Orchestration

- **Position**: 14
- **Icon**: ship
- **Prerequisites**: Section 13

### Learning Objectives

1. You can explain why container orchestration exists and what problems Kubernetes solves.
2. You can describe Kubernetes architecture: control plane (API server, etcd, scheduler, controller manager) and worker nodes (kubelet, kube-proxy, container runtime).
3. You can create and manage core workloads: Pods, Deployments, ReplicaSets, StatefulSets, DaemonSets, Jobs, CronJobs.
4. You can configure service discovery and load balancing: Services (ClusterIP, NodePort, LoadBalancer), Ingress, and DNS.
5. You can manage configuration and secrets: ConfigMaps, Secrets, environment variables, volume mounts.
6. You can configure persistent storage: PersistentVolumes, PersistentVolumeClaims, StorageClasses, and volume types.
7. You can implement RBAC: Roles, ClusterRoles, RoleBindings, ServiceAccounts.
8. You can write and manage Kubernetes manifests (YAML), use `kubectl` fluently, and understand the declarative model.
9. You can implement resource management: requests, limits, LimitRanges, ResourceQuotas, HPA (Horizontal Pod Autoscaler).
10. You can describe and implement network policies for pod-to-pod traffic control.
11. You can explain Kubernetes operators, CRDs (Custom Resource Definitions), and the operator pattern.
12. You can troubleshoot failing pods, services, and deployments using logs, events, describe, and exec.
13. You can describe the Kubernetes scheduler: how it selects nodes, affinity/anti-affinity, taints and tolerations.
14. You can explain Pod Security Standards and admission controllers.
15. You can describe the etcd data store and why it matters for cluster reliability.

### Topic Outline

1. **Why orchestration?**
   - Single-host limitations: scaling, high availability, zero-downtime deployments, service discovery
   - What orchestrators do: scheduling, scaling, networking, storage, self-healing, declarative management
   - Orchestrator landscape: Kubernetes (dominant), Docker Swarm (simple), Nomad (HashiCorp), ECS/Fargate (AWS)
   - Kubernetes distributions: upstream (kubeadm), managed (EKS, AKS, GKE), lightweight (k3s, kind, minikube)
   - CNCF ecosystem: the Cloud Native Computing Foundation and its project landscape
2. **Kubernetes architecture**
   - Control plane components:
     - API server (`kube-apiserver`): REST API, admission controllers, authentication, authorization — the front door to the cluster
     - etcd: distributed key-value store, the source of truth for all cluster state, Raft consensus
     - Scheduler (`kube-scheduler`): assigns Pods to nodes based on resource requests, affinity, taints, topology
     - Controller manager (`kube-controller-manager`): runs control loops — Deployment controller, ReplicaSet controller, Node controller, etc.
     - Cloud controller manager: integrates with cloud provider APIs for load balancers, volumes, nodes
   - Worker node components:
     - kubelet: agent on each node, manages Pod lifecycle, reports status to API server
     - kube-proxy: network rules for Service traffic (iptables/IPVS mode), pod-to-service routing
     - Container runtime: containerd (most common), CRI-O — implements CRI (Container Runtime Interface)
   - Add-ons: CoreDNS (cluster DNS), metrics-server, CNI plugins (network)
   - The declarative model: desired state in manifests → controllers reconcile actual state → convergence
   - The reconciliation loop: observe → diff → act → repeat
3. **Workload resources**
   - Pod: smallest deployable unit, one or more containers, shared network namespace and storage
     - Pod spec: containers, initContainers, volumes, restartPolicy, terminationGracePeriodSeconds
     - Multi-container patterns: sidecar, ambassador, adapter
     - Pod lifecycle: Pending → Running → Succeeded/Failed, pod phases, container states (waiting, running, terminated)
     - Probes: liveness (restart if fails), readiness (remove from service if fails), startup (delay other probes), HTTP/TCP/exec
   - ReplicaSet: ensures N pod replicas, label selectors — usually managed by Deployment
   - Deployment: declarative updates for Pods, rolling updates, rollback, revision history
     - Strategy: RollingUpdate (maxSurge, maxUnavailable), Recreate
     - `kubectl rollout status`, `rollout history`, `rollout undo`
   - StatefulSet: ordered deployment, stable network identities, stable persistent storage — for databases, distributed systems
     - Headless Service: `clusterIP: None`, DNS records per pod (`pod-0.service.namespace.svc.cluster.local`)
     - Volume claim templates: per-pod persistent storage
     - Ordered and parallel pod management
   - DaemonSet: one Pod per node (or subset of nodes), for node-level agents (logging, monitoring, networking)
   - Job: run-to-completion workloads, `completions`, `parallelism`, `backoffLimit`, `activeDeadlineSeconds`
   - CronJob: scheduled Jobs, cron syntax, `concurrencyPolicy` (Allow, Forbid, Replace), `startingDeadlineSeconds`
4. **Service discovery and networking**
   - Service types:
     - ClusterIP: internal-only, virtual IP, default
     - NodePort: expose on every node's IP at a static port (30000-32767)
     - LoadBalancer: provision cloud load balancer, external access
     - ExternalName: CNAME alias to external service
   - Service mechanics: label selectors match Pods, Endpoints/EndpointSlices track ready Pod IPs
   - DNS: `service.namespace.svc.cluster.local`, Pod DNS, headless Services
   - Ingress: HTTP/HTTPS routing, host-based and path-based rules, TLS termination
   - Ingress controllers: NGINX Ingress, Traefik, HAProxy, cloud-specific (ALB, Application Gateway)
   - Gateway API: the next-generation Ingress, HTTPRoute, GRPCRoute, Gateway classes
   - CNI (Container Network Interface): plugins that implement pod networking — Calico, Cilium, Flannel, Weave
   - Pod-to-pod networking: flat network, every pod gets an IP, pods can reach all other pods
   - Network Policies: allow/deny rules for pod-to-pod traffic, namespace isolation, default deny, ingress and egress rules
   - Service mesh concepts: Istio, Linkerd — mTLS, traffic management, observability (overview, not deep dive)
5. **Configuration and secrets**
   - ConfigMaps: key-value configuration, environment variables, volume mounts, immutable ConfigMaps
   - Secrets: base64-encoded (not encrypted at rest by default), types (Opaque, docker-registry, tls), volume mounts
   - Encryption at rest: `EncryptionConfiguration`, KMS providers for encrypting Secrets in etcd
   - Environment variables: `env`, `envFrom`, `valueFrom` (ConfigMap/Secret references), `fieldRef` (pod metadata), `resourceFieldRef`
   - Immutable ConfigMaps and Secrets: prevent accidental changes, improve performance (no watches)
   - External secret management: External Secrets Operator, Sealed Secrets, Vault integration
6. **Storage**
   - Volumes: `emptyDir`, `hostPath`, `configMap`, `secret`, `persistentVolumeClaim`, `projected`
   - PersistentVolume (PV): cluster-level storage resource, capacity, access modes (ReadWriteOnce, ReadOnlyMany, ReadWriteMany)
   - PersistentVolumeClaim (PVC): user request for storage, bound to PV
   - StorageClass: dynamic provisioning, reclaim policy (Retain, Delete), volume expansion
   - CSI (Container Storage Interface): standard for storage plugins, cloud provider CSI drivers
   - Access modes: RWO (one node), ROX (many nodes read), RWX (many nodes read/write) — depends on storage backend
   - Volume snapshots: create point-in-time copies, restore from snapshots
7. **RBAC and security**
   - RBAC components:
     - Role: namespace-scoped permissions (verbs + resources + apiGroups)
     - ClusterRole: cluster-scoped permissions
     - RoleBinding: binds Role to subjects (users, groups, ServiceAccounts) in a namespace
     - ClusterRoleBinding: binds ClusterRole to subjects cluster-wide
   - ServiceAccounts: identity for pods, mounted token, RBAC binding, `automountServiceAccountToken: false`
   - Common verbs: get, list, watch, create, update, patch, delete
   - Pod Security Standards: Privileged, Baseline, Restricted — enforced via Pod Security Admission
   - Pod Security Admission: namespace labels, modes (enforce, audit, warn)
   - Security contexts: `runAsNonRoot`, `readOnlyRootFilesystem`, `allowPrivilegeEscalation: false`, `capabilities`, `seccompProfile`
   - Admission controllers: mutating and validating, built-in (PodSecurity, ResourceQuota) and custom (webhooks)
   - OPA Gatekeeper / Kyverno: policy engines for Kubernetes, constraint templates, policy as code
8. **Resource management**
   - Resource requests: guaranteed resources, used for scheduling decisions
   - Resource limits: maximum resources, exceeded → OOMKilled (memory) or throttled (CPU)
   - LimitRange: default requests/limits per namespace, min/max constraints
   - ResourceQuota: aggregate limits per namespace (total CPU, memory, object counts)
   - QoS classes: Guaranteed (requests == limits), Burstable (requests < limits), BestEffort (no requests/limits)
   - HPA (Horizontal Pod Autoscaler): scale replicas based on CPU, memory, or custom metrics
   - VPA (Vertical Pod Autoscaler): adjust resource requests automatically
   - Pod Disruption Budgets (PDB): limit voluntary disruptions during maintenance
9. **Scheduling**
   - How the scheduler works: filtering (feasible nodes) → scoring (best node) → binding
   - Node selectors: simple label matching, `nodeSelector`
   - Node affinity: `requiredDuringSchedulingIgnoredDuringExecution`, `preferredDuringSchedulingIgnoredDuringExecution`
   - Pod affinity and anti-affinity: co-locate or separate pods based on labels
   - Taints and tolerations: nodes repel pods unless pods tolerate the taint, `NoSchedule`, `PreferNoSchedule`, `NoExecute`
   - Topology spread constraints: distribute pods across zones, nodes, or other topology domains
   - Priority and preemption: PriorityClasses, high-priority pods can evict lower-priority ones
10. **Operators and CRDs**
    - Custom Resource Definitions (CRDs): extend the Kubernetes API with custom resource types
    - Custom controllers: watch custom resources, reconcile desired state
    - The Operator pattern: CRD + controller = domain-specific automation
    - Examples: database operators (PostgreSQL, MySQL), certificate management (cert-manager), monitoring (Prometheus Operator)
    - Operator frameworks: Operator SDK (Go, Ansible, Helm), Kubebuilder, KUDO
    - Operator Lifecycle Manager (OLM): install, update, manage operators
    - Helm: package manager for Kubernetes, charts, values, templates, releases, repositories
11. **Observability and troubleshooting**
    - `kubectl` essentials:
      - `get`, `describe`, `logs`, `exec`, `port-forward`, `apply`, `delete`, `edit`, `patch`
      - `-o yaml`, `-o json`, `-o wide`, `-o jsonpath`, `--field-selector`, `-l` (label selector)
      - `kubectl explain`: built-in API reference
      - `kubectl diff`: preview changes before applying
    - Debugging workflow:
      - Pod not starting: check events (`describe`), check scheduler, check image pull, check resource availability
      - Pod crashing: check logs, check exit codes, check probes, check resource limits
      - Service not reachable: check selectors match pods, check endpoints, check network policies, check DNS
      - Persistent volume not bound: check storage class, check access modes, check capacity
    - Events: `kubectl get events --sort-by=.metadata.creationTimestamp`
    - Ephemeral debug containers: `kubectl debug` for troubleshooting running pods
    - Cluster-level tools: metrics-server, Kubernetes Dashboard, Lens, k9s
    - Logging: stdout/stderr convention, log aggregation (Fluentd/Fluent Bit → Elasticsearch/Loki), structured logging
    - Monitoring: Prometheus + Grafana stack, kube-state-metrics, node-exporter, custom metrics

### Key Concepts

- Kubernetes is a declarative system: you describe desired state, controllers make it happen
- The reconciliation loop is the core pattern: observe, diff, act, repeat
- Pods are ephemeral — design for failure, not for permanence
- Services provide stable networking for ephemeral pods — label selectors are the glue
- RBAC + Pod Security Standards + Network Policies = defense in depth inside the cluster

### Essential Commands and Tools

- `kubectl` — Kubernetes CLI (the primary tool)
- `helm` — package management
- `k9s` — terminal UI for Kubernetes
- `kind` / `minikube` / `k3s` — local Kubernetes clusters
- `kustomize` — manifest customization (built into kubectl)
- `stern` — multi-pod log tailing

### Exercises

- Set up a local Kubernetes cluster with `kind` or `minikube`, deploy a multi-tier application (frontend, backend, database) with Deployments, Services, and ConfigMaps
- Configure liveness, readiness, and startup probes for an application and observe Kubernetes behavior when probes fail
- Implement RBAC: create a ServiceAccount, Role, and RoleBinding that allows reading pods in one namespace but nothing else — verify with `kubectl auth can-i`
- Write network policies to implement default-deny in a namespace, then selectively allow traffic between specific services
- Set up HPA and generate load to observe automatic scaling, then remove load and watch scale-down
- Create a StatefulSet with persistent storage, delete a pod, verify it comes back with the same identity and data
- Deploy an application with Helm, customize values, upgrade, and rollback
- Debug a deliberately broken deployment: identify and fix issues with image pull, resource limits, probe configuration, and service selectors
- Write a CronJob that runs a database backup every 6 hours with appropriate resource limits and failure handling

### Proficiency Criteria

You can describe every control plane component and explain what happens when you `kubectl apply`. You can deploy, scale, update, and debug applications on Kubernetes confidently. You implement RBAC and network policies without referring to documentation. You understand the scheduler, probes, resource management, and the operator pattern. You can troubleshoot any common Kubernetes issue by reading events, logs, and resource descriptions.

---

## Section 15: Infrastructure as Code

- **Position**: 15
- **Icon**: blocks
- **Prerequisites**: Section 14

### Learning Objectives

1. You can explain what Infrastructure as Code is, why it matters, and the difference between declarative and imperative approaches.
2. You can describe Terraform's architecture: providers, resources, data sources, state, plan/apply workflow.
3. You can write Terraform configurations with resources, variables, outputs, locals, and expressions.
4. You can manage Terraform state: remote backends, state locking, state commands (mv, rm, import, taint), workspaces.
5. You can write and use Terraform modules: inputs, outputs, module sources, versioning, composition.
6. You can implement DRY patterns: for_each, count, dynamic blocks, and conditional resources.
7. You can manage multiple environments (dev/staging/production) with Terraform.
8. You can test Terraform configurations: `terraform validate`, `terraform plan` review, Terratest, policy testing with OPA/Sentinel.
9. You can describe Terraform provider development concepts and how providers bridge Terraform to APIs.
10. You can explain Terraform state internals: how state maps resources to real infrastructure, drift detection, state file format.
11. You can compare Terraform to other IaC tools: Pulumi, CloudFormation, ARM/Bicep, CDK, Crossplane.
12. You can implement IaC in CI/CD: automated plan, policy checks, approval gates, automated apply.
13. You can manage secrets and sensitive values in Terraform securely.
14. You can use Terraform's import and moved blocks to bring existing infrastructure under management.

### Topic Outline

1. **IaC principles**
   - What IaC solves: manual provisioning is slow, error-prone, undocumented, and unreproducible
   - Declarative vs. imperative: "what" vs. "how", convergence vs. sequence
   - Idempotency: apply the same config multiple times → same result
   - Version control: infrastructure definitions in Git, change history, code review, collaboration
   - Immutable infrastructure: replace instead of modify, golden images, disposable instances
   - Mutable vs. immutable: configuration management (Ansible, Chef, Puppet) vs. provisioning (Terraform, CloudFormation)
   - The IaC landscape:
     - Terraform / OpenTofu: multi-cloud, declarative, HCL
     - Pulumi: multi-cloud, general-purpose languages (Python, Go, TypeScript)
     - AWS CloudFormation: AWS-only, JSON/YAML
     - Azure ARM templates / Bicep: Azure-only
     - GCP Deployment Manager / Config Connector: GCP-only
     - AWS CDK / CDKTF: generate declarative config from imperative code
     - Crossplane: Kubernetes-native IaC, CRDs for cloud resources
     - Ansible: configuration management + provisioning, procedural, agentless
2. **Terraform fundamentals**
   - HCL (HashiCorp Configuration Language): blocks, arguments, expressions, comments
   - Provider architecture: plugins that translate HCL to API calls, provider registry
   - Resources: `resource "type" "name" { }`, CRUD lifecycle, dependencies (implicit and explicit `depends_on`)
   - Data sources: `data "type" "name" { }`, read existing infrastructure, reference in resources
   - Variables: `variable "name" { type, default, description, validation, sensitive }`, input methods (`-var`, `-var-file`, `TF_VAR_*`, `terraform.tfvars`)
   - Outputs: `output "name" { value, description, sensitive }`, passing data between modules and to users
   - Locals: `locals { }`, computed values, reducing repetition
   - Terraform blocks: `terraform { required_version, required_providers, backend }`
3. **The plan/apply workflow**
   - `terraform init`: download providers, initialize backend, install modules
   - `terraform validate`: syntax and configuration validation (no API calls)
   - `terraform plan`: calculate diff between desired and actual state, show create/update/destroy actions
   - `terraform apply`: execute the plan, create/modify/destroy resources
   - `terraform destroy`: remove all managed resources
   - Plan output: `+` create, `-` destroy, `~` update in-place, `-/+` destroy and recreate
   - `-target`: apply specific resources (use sparingly, not for regular workflow)
   - `-auto-approve`: skip confirmation (only in automation, never interactively)
   - Refresh: `terraform plan` includes refresh by default, detecting drift from actual state
4. **State management**
   - What state is: a JSON file mapping resource addresses to real infrastructure IDs
   - Local state: `terraform.tfstate`, fine for learning, problematic for teams
   - Remote backends: S3 + DynamoDB (AWS), Azure Storage, GCS, Terraform Cloud/HCP — shared, locked state
   - State locking: prevent concurrent modifications, lock info, force-unlock (last resort)
   - State file format: JSON, resource instances, attribute values, dependency metadata
   - State commands:
     - `terraform state list`: list all resources in state
     - `terraform state show`: detailed view of a resource
     - `terraform state mv`: rename/move resources without destroy/recreate
     - `terraform state rm`: remove resource from state (does not destroy the real resource)
     - `terraform state pull` / `push`: download/upload state (advanced operations)
   - Drift detection: plan shows differences between state and actual infrastructure
   - State file security: contains sensitive values, encrypt at rest, restrict access
5. **HCL expressions and functions**
   - Types: string, number, bool, list, map, set, object, tuple
   - String interpolation: `"Hello, ${var.name}"`
   - Conditional: `condition ? true_val : false_val`
   - `for` expressions: `[for s in var.list : upper(s)]`, filtering with `if`
   - Splat expressions: `aws_instance.example[*].id`
   - Built-in functions:
     - String: `format`, `join`, `split`, `replace`, `trimspace`, `lower`, `upper`
     - Collection: `length`, `lookup`, `merge`, `flatten`, `keys`, `values`, `zipmap`, `concat`
     - Filesystem: `file`, `filebase64`, `templatefile`, `fileset`
     - Encoding: `jsonencode`, `jsondecode`, `yamlencode`, `yamldecode`, `base64encode`, `base64decode`
     - IP: `cidrsubnet`, `cidrhost`, `cidrnetmask`
     - Type: `tostring`, `tolist`, `tomap`, `try`, `can`
   - Dynamic blocks: generate repeated nested blocks from collections
   - Type constraints: `type = list(string)`, `type = map(object({ ... }))`, `any`
6. **Resource meta-arguments**
   - `count`: create N copies, `count.index`, conditional creation (`count = var.enabled ? 1 : 0`)
   - `for_each`: create instances from a map or set, `each.key`, `each.value`, better than count for named resources
   - `depends_on`: explicit dependency when implicit is insufficient
   - `lifecycle`: `create_before_destroy`, `prevent_destroy`, `ignore_changes`, `replace_triggered_by`
   - `provider`: override default provider, provider aliases for multi-region
   - Provisioners: `local-exec`, `remote-exec`, `file` — last resort, prefer native resources
   - `moved` block: refactor resource addresses without destroying infrastructure
   - `import` block (Terraform 1.5+): declarative import of existing resources
7. **Modules**
   - What modules solve: reusability, encapsulation, composition, standardization
   - Module structure: `main.tf`, `variables.tf`, `outputs.tf`, `versions.tf`, `README.md`
   - Module sources: local paths, Terraform Registry, Git URLs, S3/GCS, HTTP
   - Module inputs: variables declared in the module, passed by the caller
   - Module outputs: values exposed by the module, consumed by the caller
   - Module versioning: `version = "~> 3.0"`, semantic versioning, version constraints
   - Root module vs. child modules: the top-level config is the root module
   - Module composition: combining multiple modules, passing outputs as inputs
   - Public modules: Terraform Registry, verified modules, community modules — evaluate before using
   - Writing good modules: single responsibility, minimal inputs, sensible defaults, documentation
8. **Multi-environment management**
   - The challenge: same infrastructure, different configurations (size, count, naming, networking)
   - Workspaces: `terraform workspace new dev`, isolated state per workspace, `terraform.workspace` variable
   - Workspace limitations: same backend, same code, different variables — works for simple cases
   - Directory-based environments: `environments/dev/`, `environments/prod/` — separate root modules, shared modules
   - Variable files: `dev.tfvars`, `prod.tfvars`, `-var-file=` flag
   - Terragrunt: DRY multi-environment management, keep backend config DRY, run modules across environments
   - Environment promotion: dev → staging → prod, same module version, different variables
9. **Testing and policy**
   - `terraform validate`: basic syntax and type checking
   - `terraform plan` review: the most important quality gate, review every change before apply
   - `terraform fmt`: consistent formatting, enforce in CI
   - Terratest: Go-based integration testing, deploy real infrastructure, validate, destroy
   - `terraform test` (native, Terraform 1.6+): HCL-based tests, mock providers, plan/apply assertions
   - Policy as code:
     - OPA (Open Policy Agent): Rego language, `conftest` for Terraform plan evaluation
     - HashiCorp Sentinel: enterprise policy framework, policy sets, enforcement levels
     - Checkov: static analysis for Terraform, security and compliance scanning
     - tflint: linting rules, provider-specific checks, custom rules
   - Cost estimation: `infracost`, catch cost surprises before apply
10. **IaC in CI/CD**
    - Pipeline stages: lint → validate → plan → policy check → (manual approval) → apply
    - Plan output as PR comment: show changes for review, `terraform plan -out=plan.tfplan`, `terraform show -json`
    - Automated apply: after approval, apply the saved plan — never re-plan before apply
    - State file protection: CI/CD runner needs credentials for backend and providers
    - OIDC authentication: GitHub Actions OIDC → cloud provider role, no long-lived credentials
    - Terraform Cloud / HCP Terraform: managed runs, policy enforcement, state management, team workflows
    - Atlantis: pull request automation for Terraform, plan on PR, apply on merge
    - Drift detection: scheduled plans to detect out-of-band changes
11. **Secrets and sensitive data**
    - `sensitive = true` on variables and outputs: prevents values from appearing in plan output
    - Sensitive values in state: state file contains actual values — protect the state file
    - Secret injection: CI/CD secrets, Vault dynamic secrets, OIDC-based credential exchange
    - `.tfvars` with secrets: never commit, add to `.gitignore`, use secret managers instead
    - Data sources for secrets: `data "vault_generic_secret"`, `data "aws_secretsmanager_secret"`, etc.
    - `terraform output -json`: be careful — sensitive outputs are shown with `-json` flag
12. **Advanced topics**
    - Provider development concepts: resource CRUD operations, schema definition, the Terraform Plugin Framework
    - State internals: resource instance objects, deposed instances, dependency graph, serial number
    - Resource addressing: `module.foo.aws_instance.bar[0]`, fully qualified addresses
    - Terraform graph: `terraform graph | dot -Tsvg`, dependency visualization
    - `terraform console`: interactive expression evaluator
    - Backend migration: moving between backends, `terraform init -migrate-state`
    - Import workflow: `terraform import`, `import {}` block, generating configuration for imported resources
    - Refactoring: `moved` blocks, `terraform state mv`, safe resource renaming
    - Performance: large state files, parallelism (`-parallelism=N`), refresh-only plans, targeted plans

### Key Concepts

- IaC makes infrastructure reproducible, reviewable, and version-controlled — treat infrastructure like software
- `terraform plan` is the most important step — never apply without reviewing the plan
- State is the bridge between your configuration and real infrastructure — protect it, lock it, back it up
- Modules enable reuse — write them well, version them, compose them
- Multi-environment management is a solved problem — use directory structure, variables, and CI/CD

### Essential Commands and Tools

- `terraform init` / `plan` / `apply` / `destroy` — core workflow
- `terraform state` — state management
- `terraform import` — bring existing resources under management
- `terraform workspace` — multi-environment state
- `terraform fmt` / `validate` — code quality
- `terraform console` — interactive expression testing
- `tflint` — linting
- `checkov` / `tfsec` — security scanning
- `infracost` — cost estimation
- `terragrunt` — DRY multi-environment management

### Exercises

- Write a Terraform configuration that provisions a virtual network, a compute instance, and a storage bucket — using variables for all environment-specific values
- Refactor inline resources into a reusable module with inputs, outputs, and a README — call the module from two environment root modules (dev and prod)
- Set up a remote backend with state locking, intentionally simulate a lock conflict, resolve it
- Use `for_each` to create multiple resources from a map, `dynamic` blocks for nested configuration, and conditional creation with `count`
- Import an existing resource into Terraform state, write the matching configuration, verify with `terraform plan` showing no changes
- Build a CI/CD pipeline that runs `terraform fmt -check`, `validate`, `plan` (output as PR comment), and `apply` after approval
- Write OPA/Rego policies that enforce tagging requirements and block overly permissive security configurations, integrate with `conftest`
- Implement a multi-environment setup using directory-based structure with shared modules, deploy dev and prod with different instance sizes and counts
- Use `terraform test` to write and run integration tests for a module

### Proficiency Criteria

You can architect a Terraform codebase for a multi-environment, multi-team organization. You understand state deeply — what it contains, how it works, how to fix it when it breaks. You write modules that are reusable, well-documented, and versioned. You implement CI/CD for Terraform with proper plan review, policy enforcement, and secure state management. You can compare Terraform to alternatives and explain when each is appropriate. You can import, refactor, and migrate Terraform configurations without fear.
