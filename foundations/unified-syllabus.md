# Unified Foundations Syllabus

## Complete Curriculum — Zero to Deep Expert

---

**Format:** MLA
**Subject:** Cloud Infrastructure and Computer Science — Unified Foundations
**Date:** 8 February 2026
**Scope:** All topics required for deep expert proficiency in cloud infrastructure, DevOps,
and core computer science — from first principles through production systems

---

## Course Description

This syllabus unifies two curricula into a single linear path: the core computer science
foundation that every CS graduate is expected to master, and the practical infrastructure
and automation skills that every cloud and DevOps professional must demonstrate. The result
is 22 sections organized into 7 phases, designed so that theory and practice reinforce each
other at every step. Mathematical and theoretical concepts are introduced immediately before
the applied sections that depend on them, maximizing retention and eliminating the gap
between understanding and doing.

Upon completion, the learner can explain internals, troubleshoot edge cases, make
architectural decisions with informed tradeoffs, and teach others — across both
theoretical computer science and production infrastructure.

---

## Curriculum Overview

### Phase 1: Orientation (Section 1)

| # | Title | Description |
|---|-------|-------------|
| 1 | Getting Started | Career landscape, learning strategy, and environment setup. |

### Phase 2: How Machines Think (Sections 2–4)

| # | Title | Description |
|---|-------|-------------|
| 2 | Mathematical Foundations I | Logic, number systems, sets, Boolean algebra, and proof techniques. |
| 3 | Introduction to Computers | Digital logic, processor architecture, hardware, memory hierarchy, data representation, and the boot process. |
| 4 | Operating Systems | Processes, scheduling, synchronization, deadlock, memory management, file systems, I/O, permissions, namespaces, and cgroups. |

### Phase 3: Working with Machines (Sections 5–7)

| # | Title | Description |
|---|-------|-------------|
| 5 | Linux | Terminal fluency, filesystem hierarchy, packages, systemd, disks, networking, and performance analysis. |
| 6 | Text Editing | Vim mastery — modes, motions, registers, macros, configuration, and the text-processing philosophy. |
| 7 | Shell Scripting | Bash automation — variables, control flow, pipes, error handling, scheduling, and practical patterns. |

### Phase 4: Programming Depth (Sections 8–11)

| # | Title | Description |
|---|-------|-------------|
| 8 | Programming Fundamentals | Python, OOP, functional programming, testing, virtual environments, and project structure. |
| 9 | Data Structures | Arrays, linked lists, stacks, queues, hash tables, trees, heaps, and graphs — implemented and analyzed. |
| 10 | Algorithms | Asymptotic analysis, sorting, searching, greedy, divide and conquer, dynamic programming, and backtracking. |
| 11 | Mathematical Foundations II | Discrete mathematics, combinatorics, graph theory, probability, and statistics. |

### Phase 5: Networks, Data, and Integration (Sections 12–16)

| # | Title | Description |
|---|-------|-------------|
| 12 | Version Control | Git internals, branching, collaboration workflows, and repository management. |
| 13 | Networking | OSI/TCP models, IP, TCP/UDP, DNS, HTTP, TLS, SSH, firewalls, overlays, and packet analysis. |
| 14 | Databases | Relational model, SQL, normalization, transactions, indexing, NoSQL, and database operations. |
| 15 | API Fundamentals | REST, authentication, data formats, client resilience, GraphQL, gRPC, and webhooks. |
| 16 | Security Fundamentals | Cryptography, PKI, authentication, authorization, secrets management, OWASP, hardening, and compliance. |

### Phase 6: Engineering and Automation (Sections 17–21)

| # | Title | Description |
|---|-------|-------------|
| 17 | Software Engineering | SDLC, design patterns, architectural patterns, code quality, build systems, and testing strategy. |
| 18 | CI/CD | Pipelines, GitHub Actions, deployment strategies, quality gates, GitOps, and pipeline security. |
| 19 | Containers | OCI spec, runtimes, Dockerfiles, multi-stage builds, networking, storage, Compose, and rootless containers. |
| 20 | Container Orchestration | Kubernetes architecture, workloads, services, storage, RBAC, scheduling, operators, and troubleshooting. |
| 21 | Infrastructure as Code | Terraform — providers, state, modules, multi-environment management, testing, policy, and CI/CD integration. |

### Phase 7: Theoretical Capstone (Section 22)

| # | Title | Description |
|---|-------|-------------|
| 22 | Theory of Computation | Automata, Turing machines, computability, and complexity classes. |

### Dependency Graph

```
1 Getting Started
└─▶ 2 Mathematical Foundations I
    └─▶ 3 Introduction to Computers
        └─▶ 4 Operating Systems
            └─▶ 5 Linux
                ├─▶ 6 Text Editing
                │   └─▶ 7 Shell Scripting
                │       └─▶ 8 Programming Fundamentals
                │           ├─▶ 9 Data Structures
                │           │   └─▶ 10 Algorithms
                │           │       └─▶ 11 Mathematical Foundations II
                │           │           └─▶ 22 Theory of Computation
                │           ├─▶ 12 Version Control
                │           │   └─▶ 18 CI/CD
                │           └─▶ 15 API Fundamentals
                └─▶ 13 Networking
                    ├─▶ 14 Databases
                    │   └─▶ 15 API Fundamentals
                    ├─▶ 15 API Fundamentals
                    └─▶ 16 Security Fundamentals
                        └─▶ 17 Software Engineering
                            └─▶ 18 CI/CD
                                └─▶ 19 Containers
                                    └─▶ 20 Container Orchestration
                                        └─▶ 21 Infrastructure as Code
```

---

## Section 1. Getting Started

### A. The Tech Career Landscape

1. What cloud computing is and why it exists
2. Role taxonomy: DevOps engineer, SRE, cloud engineer, platform engineer, infrastructure engineer
3. What these roles share vs. where they diverge
4. Day-in-the-life breakdowns for each role
5. The job market: what employers actually screen for

### B. How This Path Works

1. The 22-section structure and phase organization
2. How theory and practice sections reinforce each other
3. Time investment expectations (effort benchmarks, not calendar estimates)
4. How to use exercises, proficiency criteria, and self-assessment

### C. Setting Up Your Environment

1. Choosing a terminal emulator (Terminal.app, Windows Terminal, GNOME Terminal, Alacritty, iTerm2)
2. Installing a hypervisor (VirtualBox, UTM, QEMU/KVM, Hyper-V) or provisioning a cloud instance
3. Choosing a Linux distribution for learning (Ubuntu, Fedora, Arch — tradeoffs of each)
4. Verifying your setup: SSH into your environment, run basic commands

### D. Learning Strategy

1. Active recall vs. passive review
2. Spaced repetition for command and concept retention
3. The value of breaking things intentionally
4. Building a personal lab notebook (plain text, Markdown)
5. How to read documentation: man pages, --help, official docs, RFCs

### E. Certifications and Credentials

1. When certifications help (career switchers, HR filters, structured learning)
2. When they do not (replacing hands-on skill, credential-stacking without depth)
3. Major certification paths: AWS/Azure/GCP, CKA/CKAD, Terraform Associate, CompTIA, RHCSA
4. How to use this path as preparation for multiple certification tracks

---

## Section 2. Mathematical Foundations I

### A. Pre-Mathematics and Logic

1. Basic arithmetic and number systems (binary, octal, decimal, hexadecimal)
2. Conversions between bases and why computers use base-2
3. Order of operations and algebraic notation
4. Propositional logic: AND, OR, NOT, XOR, implication, biconditional
5. Truth tables and logical equivalences
6. Predicate logic and quantifiers (universal, existential)

### B. Sets

1. Sets, subsets, power sets, Cartesian products
2. Set operations: union, intersection, complement, difference
3. Venn diagrams and membership notation
4. Connection to data structures (sets, maps) seen later

### C. Boolean Algebra

1. Boolean algebra laws: De Morgan's, distributive, associative, commutative, identity, complement
2. Boolean expression simplification
3. Direct connection to digital logic circuits (Section 3)
4. Karnaugh maps for simplification

### D. Proof Techniques

1. Direct proof
2. Proof by contradiction
3. Proof by induction (weak and strong)
4. Why induction matters for algorithm correctness and analysis

---

## Section 3. Introduction to Computers

### A. Digital Logic

1. Logic gates: AND, OR, NOT, NAND, NOR, XOR, XNOR
2. Combinational circuits: multiplexers, demultiplexers, encoders, decoders, half-adders, full-adders, ripple-carry adders
3. Sequential circuits: SR latch, D flip-flop, JK flip-flop, T flip-flop
4. Registers and counters
5. Finite state machines: Mealy and Moore models, state diagrams, state tables
6. Programmable logic: PLDs, FPGAs

### B. Processor Architecture

1. Instruction set architecture (ISA): CISC vs. RISC
2. Assembly language concepts (x86 and ARM)
3. Instruction encoding and addressing modes (immediate, register, direct, indirect)
4. The fetch-decode-execute cycle
5. Single-cycle, multi-cycle, and pipelined datapaths
6. Pipeline hazards: structural, data, control
7. Hazard mitigation: forwarding, stalling, branch prediction
8. Superscalar execution, out-of-order execution, speculative execution
9. Virtualization extensions: VT-x, AMD-V, ring -1, VMCS, nested virtualization

### C. Hardware Components

1. CPU: cores, threads, clock speed, instruction sets (x86_64, ARM), microarchitecture
2. RAM: DRAM vs. SRAM, DDR generations, channels, ECC memory, registered vs. unbuffered
3. Storage: HDD (platters, seek time, rotational latency) vs. SSD (NAND flash, wear leveling, TRIM, NVMe vs. SATA)
4. GPU: parallel processing, CUDA/OpenCL basics
5. NIC: Ethernet speeds (1G/10G/25G/100G), hardware offloading (TCP offload, SR-IOV)
6. Motherboard: chipset, buses (PCIe lanes, bandwidth), form factors (ATX, mATX, ITX)
7. PSU: wattage, efficiency ratings (80 Plus), rails
8. Peripherals: USB standards, Thunderbolt, display interfaces

### D. Memory Hierarchy

1. Registers (sub-nanosecond) → L1 cache (1ns) → L2 (3–5ns) → L3 (10–15ns) → RAM (50–100ns) → SSD (10–100μs) → HDD (1–10ms)
2. SRAM vs. DRAM vs. Flash vs. HDD vs. SSD
3. Cache memory: direct-mapped, set-associative, fully associative
4. Cache policies: write-through, write-back, write-allocate
5. Cache replacement: LRU, FIFO, random
6. Cache coherence: MESI protocol, cache lines, false sharing
7. NUMA: non-uniform memory access, multi-socket performance implications
8. Memory consistency and coherence

### E. Data Representation

1. Bits, bytes, and words
2. Integer representation: unsigned, signed (two's complement), overflow
3. Floating-point: IEEE 754, single and double precision, why 0.1 + 0.2 != 0.3
4. Character encoding: ASCII, Unicode, UTF-8, UTF-16
5. Endianness: big-endian vs. little-endian, network byte order
6. Error detection and correction: parity bits, checksums, Hamming codes, CRC
7. Units: bits vs. bytes, KB vs. KiB (SI vs. binary prefixes)

### F. Software Layers

1. Firmware: BIOS, UEFI, embedded controller firmware, firmware updates
2. System software: operating system, device drivers, system utilities
3. Application software: userland programs, interpreted vs. compiled
4. The kernel: kernel space vs. user space, system calls as the boundary

### G. The Boot Process

1. Power-on: PSU power-good signal, CPU reset vector
2. POST: power-on self-test, beep codes, diagnostic LEDs
3. BIOS vs. UEFI: legacy vs. modern, Secure Boot, TPM
4. Partition tables: MBR (4 primary, 2TB limit) vs. GPT (128 partitions, 9.4ZB)
5. Bootloader: GRUB2 (Linux), Windows Boot Manager, multi-boot configurations
6. Kernel loading: initramfs/initrd, kernel parameters, module loading
7. Init system: SysVinit → Upstart → systemd, PID 1 responsibilities
8. Login: getty, PAM, display manager vs. TTY

### H. Virtualization and Emulation

1. Type 1 hypervisors (bare-metal): ESXi, Hyper-V, Xen, KVM
2. Type 2 hypervisors (hosted): VirtualBox, VMware Workstation, Parallels, UTM
3. Hardware-assisted virtualization: VMCS, VMEXIT/VMENTER
4. Paravirtualization: virtio drivers, performance advantages
5. Emulation vs. virtualization: QEMU in emulation mode vs. KVM-accelerated
6. Containers vs. VMs (preview of Section 19)

---

## Section 4. Operating Systems

### A. Fundamentals

1. What an operating system does: abstraction, resource management, isolation
2. OS history: batch, multiprogramming, time-sharing, modern
3. Kernel architectures: monolithic (Linux), microkernel (Minix, QNX), hybrid (Windows NT, macOS/XNU)
4. Kernel space vs. user space: ring 0 vs. ring 3, system call boundary
5. System calls: the API between user space and kernel space
6. Common syscalls: open, read, write, close, fork, exec, mmap, ioctl

### B. Process Management

1. Processes: PID, PPID, address space, file descriptors, environment, PCB
2. fork(): creating a child process, copy-on-write optimization
3. exec(): replacing the current process image
4. fork()+exec() pattern: how shells launch programs
5. Process states: running, sleeping (interruptible/uninterruptible), stopped, zombie
6. Zombie processes: what they are, why they happen, cleanup
7. Orphan processes: reparenting to PID 1
8. Context switching: what it costs, when it happens

### C. Threads

1. Threads: shared address space, POSIX threads (pthreads)
2. Kernel threads vs. user threads
3. Thread models: 1:1, M:N
4. Thread safety: mutexes, semaphores, race conditions, deadlocks

### D. Process Synchronization

1. Race conditions and critical sections
2. Synchronization primitives: mutexes, semaphores, monitors, condition variables
3. Classic problems: producer-consumer, readers-writers, dining philosophers
4. Spinlocks vs. blocking locks

### E. Deadlock

1. Four conditions: mutual exclusion, hold and wait, no preemption, circular wait
2. Deadlock detection: wait-for graphs, cycle detection
3. Deadlock prevention: breaking one of the four conditions
4. Deadlock avoidance: Banker's algorithm
5. Deadlock recovery: process termination, resource preemption

### F. CPU Scheduling

1. Preemptive vs. cooperative scheduling
2. Scheduling algorithms: FCFS, SJF, SRTF, round-robin, priority, multilevel feedback queue
3. Linux CFS: virtual runtime, red-black tree, nice values, weight
4. Real-time scheduling: SCHED_FIFO, SCHED_RR, SCHED_DEADLINE
5. CPU affinity: pinning processes to cores, taskset
6. Load average: what the three numbers mean, interpretation
7. Signals: SIGTERM, SIGKILL, SIGHUP, SIGINT, SIGCHLD — handling and traps

### G. Memory Management

1. Physical vs. virtual memory: why the abstraction exists
2. Address spaces and memory protection
3. Contiguous allocation and segmentation
4. Paging: page size (4KB default, huge pages 2MB/1GB), page tables, multi-level page tables, inverted page tables
5. TLB: caching page table entries, TLB flushes
6. Memory management unit (MMU)
7. Page faults: minor vs. major
8. Demand paging and page fault handling
9. Page replacement algorithms: FIFO, LRU, optimal, clock
10. Thrashing and working set model
11. Swap: partitions vs. files, swappiness, zswap, zram
12. OOM killer: trigger conditions, scoring, oom_score_adj
13. Memory overcommit: settings in /proc/sys/vm/overcommit_memory
14. Huge pages: transparent huge pages vs. explicit, when they help
15. Memory-mapped files: mmap(), shared memory
16. NUMA awareness: allocation policies, numactl

### H. File Systems

1. What a file system does: map names to data on disk
2. File concepts: names, types, attributes, operations
3. Directory structures: single-level, two-level, tree, acyclic graph
4. Inodes: metadata, inode table, inode exhaustion
5. Dentries: directory entries, the dcache
6. Superblock: file system metadata, mount options
7. File system implementation: allocation methods (contiguous, linked, indexed)
8. Free space management: bitmaps, linked lists
9. Journaling: write-ahead logging, journal modes (writeback, ordered, journal)
10. File system types: ext4, XFS, Btrfs, ZFS, NTFS, tmpfs, procfs, sysfs
11. VFS: the abstraction layer supporting multiple file systems
12. Hard links vs. symbolic links: inode sharing vs. path pointers
13. File descriptors: stdin(0), stdout(1), stderr(2), /proc/self/fd, limits

### I. I/O Systems

1. I/O hardware: ports, buses, controllers
2. I/O techniques: programmed I/O, interrupt-driven I/O, DMA
3. Device drivers and their architecture
4. Disk scheduling: FCFS, SSTF, SCAN, C-SCAN, LOOK
5. I/O buffering and caching

### J. Permissions and Access Control

1. Unix DAC: owner, group, others — read, write, execute
2. Numeric (octal) vs. symbolic notation
3. umask: default permission mask, calculating resulting permissions
4. Special permissions: setuid, setgid, sticky bit
5. ACLs: getfacl, setfacl
6. Linux capabilities: fine-grained root privileges (CAP_NET_BIND_SERVICE, CAP_SYS_ADMIN)
7. SELinux and AppArmor: MAC overview, enforcing vs. permissive

### K. Namespaces and Cgroups

1. Linux namespaces: PID, network, mount, UTS, IPC, user, cgroup — what each isolates
2. unshare and nsenter
3. cgroups v2: unified hierarchy, controllers (cpu, memory, io, pids)
4. Resource limits: CPU shares/quota, memory limits (soft/hard), IO weight
5. Why namespaces + cgroups = containers (preview of Section 19)

### L. Kernel Modules and Introspection

1. Kernel modules: lsmod, modprobe, insmod, rmmod, modinfo
2. /proc: process and kernel info (cpuinfo, meminfo, loadavg, pid subdirectories)
3. /sys: device and driver info, sysfs hierarchy
4. strace: tracing system calls, interpreting output
5. seccomp: restricting available syscalls, seccomp-bpf profiles

### M. Inter-Process Communication

1. Pipes: anonymous (|) and named (FIFOs)
2. Unix domain sockets: AF_UNIX, local IPC
3. Shared memory: System V (shmget/shmat) and POSIX (shm_open)
4. Message queues: System V vs. POSIX
5. Signals: asynchronous notification, default handlers
6. D-Bus: desktop and systemd IPC

---

## Section 5. Linux

### A. The Filesystem Hierarchy

1. FHS: what it defines and why
2. /, /bin, /sbin, /etc, /var, /home, /root, /tmp, /run
3. /usr (bin, lib, share), the usr merge
4. /opt, /dev (block, character, /dev/null, /dev/zero, /dev/urandom)
5. /proc, /sys (virtual filesystems)
6. /mnt, /media, /boot

### B. File Operations

1. Basics: ls, cp, mv, rm, mkdir, touch, cat, less, more, head, tail
2. ls deep dive: -l, -a, -R, -h, -i, -S, -t
3. Wildcards and globbing: *, ?, [abc], [a-z], {foo,bar}, extended globbing
4. find: by name, type, size, mtime, user, permissions, -exec, -delete, -print0 with xargs -0
5. locate/plocate: updatedb, when to use over find
6. stat, file (magic bytes), ln (hard and symbolic)
7. tar: create, extract, list, compression (gzip, bzip2, xz, zstd)
8. rsync: --archive, --delete, --dry-run, --exclude
9. dd: disk duplication, block size tuning, status=progress

### C. Text Processing Pipeline

1. grep: basic and extended regex, -r, -l, -c, -v, -i
2. sed: substitution, deletion, in-place, address ranges
3. awk: field processing, BEGIN/END blocks, pattern matching
4. sort, uniq, cut, tr, wc, diff, tee
5. Multi-stage pipelines for data extraction and transformation

### D. User and Group Management

1. /etc/passwd, /etc/shadow, /etc/group: format and fields
2. useradd/adduser, usermod (-aG, lock/unlock), userdel (-r)
3. groupadd, groupmod, groupdel
4. passwd, su vs. sudo, visudo, /etc/sudoers, sudoers.d
5. PAM: modular authentication, common modules (pam_unix, pam_pwquality, pam_limits)

### E. Package Management

1. Concepts: repositories, dependencies, metadata, signing
2. apt (Debian/Ubuntu): update, install, remove, purge, search, show, sources.list, PPAs
3. dnf (Fedora/RHEL): install, remove, search, info, history, COPR
4. pacman (Arch): -S, -R, -Ss, -Qi, -Syu, AUR
5. Package formats: .deb, .rpm, .pkg.tar.zst
6. Low-level tools: dpkg, rpm
7. Universal packages: Snap, Flatpak, AppImage — tradeoffs
8. Building from source: ./configure && make && make install, checkinstall

### F. systemd

1. Unit files: service, timer, socket, mount, target — anatomy
2. systemctl: start, stop, restart, reload, enable, disable, status, daemon-reload
3. Writing custom service units: [Unit], [Service], [Install], ExecStart, Restart, WantedBy
4. Service types: simple, forking, oneshot, notify, idle
5. Targets: multi-user.target, graphical.target, rescue.target
6. Timers: monotonic vs. realtime, OnCalendar syntax
7. journalctl: -u, -f, --since, --until, -p, persistent logging
8. systemd-analyze: boot time, blame, critical-chain

### G. Disk Management

1. Partitioning: fdisk (MBR), gdisk (GPT), parted (both)
2. File system creation: mkfs.ext4, mkfs.xfs, mkfs.btrfs
3. Mounting: mount, umount, /etc/fstab, mount options (noexec, nosuid, nodev, ro)
4. UUID vs. device path: blkid
5. LVM: PV, VG, LV concepts, creating and resizing, snapshots
6. RAID: levels (0, 1, 5, 6, 10), mdadm, software vs. hardware
7. Swap: mkswap, swapon/swapoff, files vs. partitions
8. Disk monitoring: smartctl, iostat, iotop

### H. Networking on Linux

1. ip: addr, route, link, neigh
2. ss: socket statistics, filtering by state/port/protocol
3. nmcli/nmtui: NetworkManager CLI/TUI
4. /etc/hosts, /etc/resolv.conf, /etc/nsswitch.conf
5. hostnamectl, ping, traceroute/tracepath, mtr
6. dig/nslookup, curl/wget
7. Firewalls: iptables basics, nftables, ufw/firewalld

### I. Performance Analysis

1. vmstat, iostat, sar, mpstat, pidstat
2. perf basics: stat, top, record, report
3. /proc deep dive: cpuinfo, meminfo, loadavg, [pid]/status, [pid]/maps, [pid]/fd
4. /sys deep dive: class, block, fs/cgroup
5. Kernel parameter tuning: sysctl, /etc/sysctl.conf, common parameters

### J. Environment and Shell Configuration

1. Environment variables: export, env, printenv, PATH, HOME, SHELL, LANG
2. Shell startup files: /etc/profile, ~/.bash_profile, ~/.bashrc, ~/.profile — login vs. non-login, interactive vs. non-interactive
3. source, shell options (set -e, -u, -o pipefail, shopt)
4. Aliases and functions
5. XDG Base Directory Specification

---

## Section 6. Text Editing

### A. Why Terminal Text Editors Matter

1. The Unix philosophy: text as a universal interface
2. Every server has vi — ubiquity and availability
3. SSH + terminal editor = editing files on any machine
4. Vim vs. Nano vs. Emacs: philosophies
5. Alternatives: Neovim, Helix, Micro

### B. Vim's Modal Model

1. Normal mode: the default, thinking and navigating
2. Insert mode: i, I, a, A, o, O, c, s
3. Visual mode: character (v), line (V), block (Ctrl-v)
4. Command-line mode: :commands, /search, !shell
5. Replace mode: R for overwrite editing
6. Why modes: efficiency argument

### C. Navigation

1. Character: h, j, k, l
2. Word: w, b, e, W, B, E
3. Line: 0, ^, $, g_
4. Screen: H, M, L, Ctrl-u, Ctrl-d, Ctrl-f, Ctrl-b, zt, zz, zb
5. File: gg, G, :{line}
6. Search: /pattern, ?pattern, n, N, *, #
7. Marks: ma, 'a, `a, '', jump list (Ctrl-o, Ctrl-i), change list (g;, g,)
8. Paragraphs and sentences: {, }, (, )

### D. Operators and Text Objects

1. Operators: d, c, y, >, <, =, gU/gu
2. Operator + motion grammar: d3w, ciw, y$, >}
3. Text objects: iw, aw, i", a(, it, ip
4. The dot command (.): repeat the last change
5. Counts: numeric prefixes multiply actions

### E. Registers

1. Unnamed register (""), named registers ("a–"z), uppercase appends
2. System clipboard: "+ and "*
3. Expression register ("=), read-only registers ("%, "#, "., ":)
4. Black hole register ("_), numbered registers ("0–"9)

### F. Search and Replace

1. /pattern, ?pattern, \v (very magic), character classes, quantifiers, groups, backreferences
2. :s/old/new/ (current line), :%s/old/new/g (entire file)
3. Flags: g, c, i, n
4. :g/pattern/command (global), :v/pattern/command (inverse global)

### G. Macros

1. Recording: qa (start), q (stop)
2. Playback: @a, @@, 10@a
3. Recursive macros, editing macros via register paste
4. Practical use cases: reformatting, batch operations

### H. Buffers, Windows, and Tabs

1. Buffers: :ls, :b{n}, :bnext, :bprev, :bd
2. Windows: :split, :vsplit, Ctrl-w commands
3. Tabs: :tabnew, :tabnext, gt, gT
4. Argument list: :args, :argdo
5. Quickfix list: :copen, :cnext, :cprev

### I. Configuration

1. Settings: number, relativenumber, expandtab, shiftwidth, ignorecase, smartcase, incsearch, hlsearch
2. Key mappings: nnoremap, inoremap, vnoremap, leader key
3. Autocommands: autocmd FileType, autocmd BufWritePre
4. Neovim: init.lua, Lua configuration, built-in LSP

### J. Non-Interactive Text Editing

1. sed: stream editing for automation
2. awk: field-based text processing
3. envsubst, m4, jinja2: template-based text generation
4. When to use Vim vs. sed vs. awk vs. a script

---

## Section 7. Shell Scripting

### A. Script Fundamentals

1. Shebang: #!/bin/bash vs. #!/usr/bin/env bash
2. Execution: chmod +x, ./script.sh, bash script.sh, source script.sh
3. Exit codes: $?, exit 0, exit 1, convention
4. Script structure: argument parsing, validation, main logic, cleanup
5. Shellcheck: static analysis, common pitfalls

### B. Variables and Data Types

1. Assignment: VAR=value (no spaces)
2. Quoting: single (literal), double (interpolation), none (word splitting + glob)
3. Parameter expansion: ${VAR}, ${VAR:-default}, ${VAR:=default}, ${VAR:?error}, ${VAR:+alt}
4. String operations: ${#VAR}, ${VAR#pattern}, ${VAR%pattern}, ${VAR/old/new}, ${VAR^^}, ${VAR,,}
5. Arrays: arr=(a b c), ${arr[@]}, ${#arr[@]}, iteration
6. Associative arrays: declare -A, iteration over keys and values
7. Arithmetic: $(( )), integer-only, bc/awk for floating-point
8. Special variables: $0, $1–$9, $#, $@ vs. $*, $$, $!, $_
9. readonly, local

### C. Conditionals

1. if/elif/else/fi
2. test / [ ]: POSIX test — string, integer, file tests
3. [[ ]]: Bash extended test — pattern matching, regex (=~), no word splitting
4. String tests: -z, -n, ==, !=
5. Integer tests: -eq, -ne, -lt, -le, -gt, -ge
6. File tests: -f, -d, -e, -r, -w, -x, -s, -L
7. case statements: patterns, |, *, ;;, ;&
8. Short-circuit: && and ||

### D. Loops

1. for var in list: words, files, command output, brace expansion
2. C-style for: for ((i=0; i<10; i++))
3. while read -r line: line-by-line processing, IFS considerations
4. while true, until: indefinite loops with break
5. select: menu-driven interaction
6. Loop control: break, continue, break N
7. Safe file iteration: spaces, special characters, nullglob

### E. I/O

1. File descriptors: 0 (stdin), 1 (stdout), 2 (stderr)
2. Output redirection: >, >>, 2>, &>, 2>&1
3. Input redirection: <, here documents (<<EOF), here strings (<<<)
4. Pipes: |, PIPESTATUS, set -o pipefail
5. Process substitution: <(cmd), >(cmd)
6. tee, /dev/null, named pipes (mkfifo)
7. exec: redirect FDs for the entire script

### F. Functions

1. Declaration: function name { } vs. name() { }
2. Arguments: $1, $2, $@, $# (scoped to function)
3. Return values: return N (exit code), stdout capture for data
4. Local variables: local var=value, dynamic scoping
5. Libraries: source lib.sh
6. Recursion: possible, no tail-call optimization

### G. Error Handling and Debugging

1. set -e (errexit), set -u (nounset), set -o pipefail
2. set -x (xtrace), bash -x script.sh
3. The defensive preamble: set -euo pipefail
4. trap: EXIT, ERR, INT, TERM — cleanup handlers
5. Error messages to stderr: echo "error" >&2
6. PS4='+ ${BASH_SOURCE}:${LINENO}: ' for debug output

### H. Scheduling

1. Cron: syntax (minute, hour, day, month, weekday), crontab -e, crontab -l
2. Special strings: @reboot, @hourly, @daily, @weekly, @monthly
3. Cron pitfalls: limited PATH, no TTY, overlapping runs, timezone issues
4. systemd timers: OnCalendar, OnBootSec, advantages over cron
5. at: one-time scheduled execution
6. flock: preventing concurrent execution

### I. Practical Patterns

1. Argument parsing: getopts, manual while+case, shift
2. Configuration files: sourcing .env, envsubst
3. Temporary files: mktemp, mktemp -d, cleanup with trap
4. Logging: timestamps, log levels, logger (syslog)
5. Lock files: flock, PID files
6. Working with JSON: jq basics (.key, .[0], pipes, select, map, @csv)
7. POSIX vs. Bash: which features are Bash-only

---

## Section 8. Programming Fundamentals

### A. Python Core

1. Installation: system Python vs. managed (pyenv, deadsnakes PPA)
2. REPL: help(), dir(), type()
3. Syntax: indentation-based blocks
4. Variables: dynamic typing, name binding, everything is an object
5. Numbers: int (arbitrary precision), float (IEEE 754), complex
6. Strings: f-strings, .format(), .join(), .split(), .strip(), slicing, immutability
7. Booleans: True/False, truthy/falsy, short-circuit
8. None: is None (not == None)
9. PEP 8: style guide, black and ruff formatters

### B. Data Structures (Built-in)

1. Lists: ordered, mutable, append, extend, slicing, shallow vs. deep copy
2. Tuples: ordered, immutable, named tuples
3. Dicts: O(1) lookup, .get(), .setdefault(), .items(), dict comprehensions, defaultdict, Counter
4. Sets: O(1) membership, union, intersection, difference
5. Choosing the right structure
6. Deque: collections.deque, O(1) both ends
7. Heapq: priority queue operations

### C. Control Flow

1. if/elif/else, ternary expression
2. for loops: range(), enumerate(), zip()
3. while, break, continue, else clause
4. Match statement (Python 3.10+)
5. Iterators and generators: __iter__/__next__, yield, generator expressions, lazy evaluation
6. itertools: chain, islice, groupby, product, permutations, combinations

### D. Functions

1. Definition, parameters, return values
2. Arguments: positional, keyword, default, *args, **kwargs, keyword-only, positional-only
3. Type hints: int, str, list[int], Optional, Union, typing module
4. Docstrings: Google, NumPy, Sphinx styles
5. First-class functions: passing and returning functions
6. Lambda: anonymous functions, sort keys, callbacks
7. Closures: capturing variables, nonlocal
8. Decorators: @decorator syntax, functools.wraps, @staticmethod, @classmethod, @property
9. Scope: LEGB rule, global keyword

### E. Object-Oriented Programming

1. Classes: __init__, self, instance vs. class attributes
2. Methods: instance, @classmethod, @staticmethod
3. Encapsulation and access conventions (_, __)
4. Dunder methods: __repr__, __str__, __eq__, __hash__, __len__, __getitem__, __iter__, __enter__/__exit__
5. Properties: @property, getter/setter/deleter
6. Inheritance: single, multiple, MRO, super()
7. Polymorphism: method overriding
8. Abstract base classes: abc.ABC, @abstractmethod
9. Dataclasses: @dataclass, field(), frozen=True
10. Protocols: typing.Protocol, structural subtyping
11. Composition vs. inheritance
12. SOLID principles

### F. Functional Programming

1. Pure functions and side effects
2. Immutability and persistent data structures
3. First-class and higher-order functions
4. Lambda expressions and closures
5. Map, filter, reduce, and fold
6. Pattern matching (match/case)

### G. Additional Paradigms

1. Imperative and procedural programming
2. Event-driven programming
3. Declarative programming
4. Concurrent programming models: threading, multiprocessing, async/await preview

### H. Error Handling

1. try/except/else/finally
2. Built-in exceptions: ValueError, TypeError, KeyError, FileNotFoundError, hierarchy
3. Custom exceptions: subclassing Exception
4. EAFP vs. LBYL
5. Context managers: with, __enter__/__exit__, contextlib.contextmanager
6. Exception chaining: raise ... from ...

### I. File I/O and Data Formats

1. open(), read(), write(), readlines(), with for automatic closing
2. Modes: r, w, a, rb, wb
3. pathlib: Path objects, / operator, .exists(), .glob(), .read_text(), .write_text()
4. JSON: json.loads(), json.dumps(), json.load(), json.dump()
5. YAML: pyyaml, yaml.safe_load()
6. CSV: csv.reader, csv.DictReader, csv.writer, csv.DictWriter
7. TOML: tomllib (3.11+)
8. Encoding: always specify encoding='utf-8'

### J. Virtual Environments and Packages

1. Why isolation matters: dependency conflicts, reproducibility
2. venv: creation, activation, deactivation
3. pip: install, freeze, requirements.txt
4. pyproject.toml: modern packaging
5. uv: fast alternative to pip
6. Package managers comparison: pip, pipenv, poetry, uv

### K. Testing

1. pytest: test discovery, assert, test functions
2. Fixtures: @pytest.fixture, scope
3. Parametrize: @pytest.mark.parametrize
4. Mocking: unittest.mock, patch, MagicMock
5. Coverage: pytest-cov, what coverage misses
6. Test organization: tests/ directory, conftest.py

### L. Standard Library

1. os, sys: environment, platform, exit
2. pathlib: modern path handling
3. subprocess: run(), capturing output, check=True, shell=True risks
4. argparse: CLI parsing, subcommands
5. logging: levels, handlers, formatters, basicConfig
6. re: regular expressions
7. datetime: date, time, timedelta, timezone-aware vs. naive, zoneinfo
8. collections: defaultdict, Counter, namedtuple, deque
9. functools: lru_cache, partial, reduce, wraps
10. shutil: copytree, rmtree, disk_usage

### M. Python Internals

1. Execution model: source → bytecode (.pyc) → CPython VM
2. GIL: what it is, why it exists, threading implications
3. Memory: reference counting + generational gc
4. Threading vs. multiprocessing: concurrent.futures
5. Async: asyncio, async/await preview

### N. Project Structure

1. Modules: import, from ... import, relative imports
2. Packages: __init__.py, nested packages
3. Entry points: if __name__ == '__main__', __main__.py, console scripts
4. Project layout: src/ vs. flat
5. .gitignore for Python

---

## Section 9. Data Structures

### A. Fundamental Structures

1. Arrays: static and dynamic, multidimensional, how Python lists work underneath
2. Linked lists: singly, doubly, circular — insert, delete, search, reverse
3. Stacks: operations (push, pop, peek), applications (expression evaluation, balanced parentheses, backtracking, call stack)
4. Queues: standard, circular, double-ended (deque), priority queues
5. Hash tables: hash functions, collision resolution (chaining, open addressing), load factor, rehashing, amortized O(1)

### B. Trees

1. Binary trees: traversals (in-order, pre-order, post-order, level-order)
2. Binary search trees: insertion, deletion, search, in-order successor
3. Balanced trees: AVL trees (rotations, balance factor), red-black trees (properties, recoloring)
4. B-trees and B+ trees: disk-based storage, database indexes, branching factor
5. Heaps: min-heap, max-heap, heapify, insert, extract-min/max, heapsort, priority queue implementation

### C. Graphs

1. Representations: adjacency matrix, adjacency list, edge list — tradeoffs
2. Directed vs. undirected, weighted vs. unweighted
3. Breadth-first search (BFS): queue-based, level-order, shortest path in unweighted graphs
4. Depth-first search (DFS): stack-based/recursive, cycle detection, connected components
5. Topological sorting: Kahn's algorithm, DFS-based, DAG requirement
6. Shortest path: Dijkstra's (non-negative weights), Bellman-Ford (negative weights), Floyd-Warshall (all pairs)
7. Minimum spanning tree: Kruskal's (union-find), Prim's (priority queue)

---

## Section 10. Algorithms

### A. Algorithm Analysis

1. Asymptotic notation: Big-O, Big-Omega, Big-Theta
2. Best, worst, and average case analysis
3. Space complexity
4. Recurrence relations and the Master Theorem
5. Amortized analysis: aggregate, accounting, potential methods
6. Lower bounds and decision trees

### B. Sorting and Searching

1. Comparison sorts: bubble, selection, insertion, merge, quick, heap — with complexity and stability analysis
2. Non-comparison sorts: counting, radix, bucket
3. Binary search and its variants
4. Order statistics: quickselect, median of medians
5. External sorting (data larger than memory)
6. Stability, adaptivity, and in-place properties

### C. Algorithm Design Paradigms

1. Brute force and exhaustive search
2. Greedy algorithms: activity selection, Huffman coding, fractional knapsack — proving greedy correctness
3. Divide and conquer: merge sort, Strassen's matrix multiplication, closest pair — recurrence analysis
4. Dynamic programming: memoization vs. tabulation, optimal substructure, overlapping subproblems
5. Classic DP problems: 0/1 knapsack, longest common subsequence, edit distance, matrix chain multiplication, coin change
6. Backtracking: N-queens, Sudoku, constraint satisfaction — pruning strategies

---

## Section 11. Mathematical Foundations II

### A. Discrete Mathematics

1. Relations and functions: injective, surjective, bijective
2. Equivalence relations and partial orders
3. Combinatorics: permutations, combinations, pigeonhole principle
4. Binomial theorem and Pascal's triangle
5. Recurrence relations: solving with substitution, recursion trees, Master Theorem (revisited with formal proof)
6. Principle of inclusion-exclusion
7. Graph theory formalism: vertices, edges, degree, paths, cycles, connectivity
8. Trees, spanning trees, graph traversals (formalized)
9. Graph coloring, planarity, Euler/Hamilton paths
10. Modular arithmetic and congruences
11. Number theory: primes, GCD, LCM, Euclidean algorithm (connects to cryptography in Section 16)
12. Boolean algebra formalism (revisited with deeper treatment)

### B. Probability and Statistics

1. Sample spaces, events, axioms of probability
2. Conditional probability and Bayes' theorem
3. Random variables: discrete and continuous
4. Probability distributions: binomial, Poisson, Gaussian, uniform, exponential
5. Expected value, variance, standard deviation
6. Joint distributions and independence
7. Law of large numbers and central limit theorem

---

## Section 12. Version Control

### A. Version Control Concepts

1. What version control solves: history, collaboration, branching, rollback
2. Centralized (SVN) vs. distributed (Git, Mercurial): why distributed won
3. Git's design principles: speed, data integrity (SHA-1), distributed, cheap branching

### B. Git's Object Model

1. Blob: file content, content-addressable by hash
2. Tree: directory listing, maps names to blobs and trees
3. Commit: snapshot pointer, parent(s), author, committer, message
4. Tag: named pointer, annotated vs. lightweight
5. The DAG: directed acyclic graph of commits
6. git cat-file, git ls-tree, git rev-parse: inspecting objects
7. Pack files: compression for storage and transfer

### C. Basic Workflow

1. git init, git clone
2. The three areas: working directory, staging area (index), repository
3. git add: staging changes, hunks with -p
4. git commit: -m, --amend
5. git status, git diff (unstaged, --staged, between commits)
6. git log: --oneline, --graph, --all, --author, --since, --grep
7. git show: inspect a commit
8. Commit messages: imperative mood, 50-char subject, 72-char body wrap

### D. Branching and Merging

1. Branches: lightweight pointers, git branch, git switch
2. Creating branches: git switch -c
3. Merging: fast-forward vs. three-way merge, merge commits
4. Merge conflicts: conflict markers, resolution workflow
5. git mergetool: vimdiff, meld, VS Code
6. Branch deletion: -d (safe) vs. -D (force)

### E. Rewriting History

1. git rebase: replay commits on a new base, linear history
2. Interactive rebase: pick, squash, fixup, reword, edit, drop
3. git cherry-pick: apply specific commits
4. git revert: undo a commit safely for shared history
5. git reset: --soft, --mixed, --hard
6. The golden rule: never rebase shared commits
7. git commit --fixup and git rebase --autosquash

### F. Recovery

1. git reflog: where HEAD has been, last-resort recovery
2. Recovering lost commits: reflog → checkout or cherry-pick
3. Recovering from bad rebase: ORIG_HEAD or reflog
4. git fsck --lost-found: dangling objects
5. git gc and object pruning

### G. Working with Remotes

1. Remote: bookmark to another repository, origin convention
2. git remote: add, -v, rename, remove
3. git fetch: download without merging
4. git pull: fetch + merge (or --rebase)
5. git push: -u for upstream, --force-with-lease
6. Tracking branches, git branch -vv
7. Forks: personal copies, upstream remotes, keeping in sync

### H. Collaboration Workflows

1. Pull requests (GitHub) / Merge requests (GitLab)
2. Code review: commenting, requesting changes, approving
3. Issues: bug reports, feature requests, linking PRs
4. Branch protection: required reviews, status checks
5. CODEOWNERS: automatic review assignment
6. Branching strategies: GitHub Flow, Git Flow, trunk-based development — tradeoffs

### I. Advanced Git

1. .gitignore: patterns, negation, global gitignore
2. git stash: stash, pop, list, apply, drop, named stashes
3. git bisect: binary search for bugs, automated with git bisect run
4. git blame: line-by-line attribution, -w, -C
5. git worktree: multiple working trees
6. Git hooks: pre-commit, commit-msg, pre-push, pre-commit framework
7. git submodule vs. git subtree
8. Signing commits: GPG/SSH signatures
9. git-lfs: large file storage

---

## Section 13. Networking

### A. Network Foundations

1. Network types: LAN, WAN, MAN, PAN
2. Network topologies: bus, star, ring, mesh
3. Circuit switching vs. packet switching
4. Bandwidth, latency, throughput, jitter
5. Standards bodies: IEEE, IETF, W3C

### B. Network Models

1. OSI model: physical, data link, network, transport, session, presentation, application
2. TCP/IP model: network interface, internet, transport, application
3. Encapsulation: data → segment → packet → frame
4. PDUs: frame (L2), packet (L3), segment/datagram (L4)

### C. Physical and Data Link Layer

1. Transmission media: copper, fiber optic, wireless
2. Signal encoding and modulation
3. Ethernet (IEEE 802.3): frame format, CSMA/CD
4. MAC addressing: 48-bit, OUI
5. Switches: MAC address table, flooding, learning, forwarding
6. VLANs: 802.1Q tagging, trunk ports, access ports
7. ARP: address resolution, ARP table, ARP spoofing
8. Wi-Fi (IEEE 802.11): standards, channels, frequencies, WPA2/WPA3
9. Error detection: CRC, checksums
10. Flow control and sliding window protocols
11. MTU: jumbo frames, fragmentation, path MTU discovery

### D. IP Addressing and Subnetting

1. IPv4: 32-bit, dotted-decimal, address classes (historical)
2. CIDR: variable-length subnet masks, /24, /16, /8
3. Subnetting: network address, broadcast address, host range, calculation
4. Private ranges: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 (RFC 1918)
5. Special addresses: 127.0.0.0/8, 169.254.0.0/16, 0.0.0.0, 255.255.255.255
6. DHCP: discover, offer, request, acknowledge
7. IPv6: 128-bit, :: abbreviation, link-local (fe80::), global unicast, SLAAC, DHCPv6
8. IPv4 vs. IPv6: dual-stack, transition mechanisms
9. IP fragmentation and reassembly
10. Multicast and anycast

### E. Transport Layer

1. TCP: connection-oriented, reliable, ordered
2. Three-way handshake: SYN → SYN-ACK → ACK
3. Sequence numbers and acknowledgments
4. Flow control: sliding window, window scaling
5. Congestion control: slow start, congestion avoidance, AIMD, fast retransmit, fast recovery, TCP algorithms (Reno, Cubic, BBR)
6. Connection teardown: FIN → ACK → FIN → ACK, TIME_WAIT
7. TCP options: MSS, timestamps, SACK
8. UDP: connectionless, unreliable, low overhead
9. UDP use cases: DNS, DHCP, VoIP, gaming, streaming, QUIC
10. Ports: well-known (0–1023), registered (1024–49151), ephemeral (49152–65535)
11. Common ports: 22, 53, 80, 443, 25/587, 3306, 5432
12. Sockets: IP + port, AF_INET/AF_INET6, SOCK_STREAM/SOCK_DGRAM
13. Multiplexing and demultiplexing

### F. DNS

1. What DNS does: name → IP resolution
2. Hierarchy: root → TLD → second-level → subdomains
3. Resolution: stub resolver → recursive resolver → root → TLD → authoritative
4. Record types: A, AAAA, CNAME, MX, NS, TXT, SRV, PTR, SOA, CAA
5. TTL: caching, propagation delays
6. Zones and zone files: SOA records, zone transfers (AXFR/IXFR)
7. DNS security: DNSSEC, DoH, DoT
8. Split-horizon DNS

### G. HTTP

1. HTTP/1.1: text-based, persistent connections, pipelining
2. Methods: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS — semantics and idempotency
3. Headers: Host, Content-Type, Authorization, Accept, Cache-Control, User-Agent, Cookie/Set-Cookie
4. Status codes: 1xx, 2xx, 3xx, 4xx, 5xx — common codes (200, 201, 301, 400, 401, 403, 404, 500, 502, 503)
5. HTTP/2: binary framing, multiplexing, HPACK header compression, server push
6. HTTP/3: QUIC (UDP-based), 0-RTT, no head-of-line blocking
7. Cookies: Secure, HttpOnly, SameSite
8. Caching: Cache-Control, ETag, CDN caching
9. Email protocols: SMTP, IMAP, POP3
10. FTP and SFTP

### H. Routing

1. Routing table: destination, gateway, interface, metric
2. Default gateway
3. Static routes: ip route add
4. Dynamic routing: distance vector (RIP), link state (OSPF), path vector (BGP)
5. NAT: SNAT, DNAT, PAT, traversal challenges
6. Port forwarding
7. ICMP and diagnostic tools: ping, traceroute/tracepath, mtr

### I. Network Overlays

1. Why overlays: limited VLANs, multi-tenant isolation, container/VM mobility
2. VXLAN: UDP encapsulation, 24-bit VNI, VTEP
3. GRE: point-to-point tunnels
4. IPsec: site-to-site VPN, tunnel vs. transport mode, IKE
5. WireGuard: modern VPN, simple configuration
6. Software-defined networking: control plane vs. data plane separation

### J. TLS and Encryption in Transit

1. TLS handshake: ClientHello → ServerHello → certificate → key exchange → encrypted
2. Certificate chain: root CA → intermediate CA → leaf
3. Certificate validation: hostname, expiry, chain of trust, revocation (CRL, OCSP)
4. Cipher suites: key exchange (ECDHE), authentication (RSA, ECDSA), encryption (AES-GCM), hash (SHA-256)
5. TLS 1.2 vs. 1.3: 1.3 has faster handshake, fewer cipher suites
6. mTLS: mutual authentication, service-to-service
7. openssl: key generation, CSRs, self-signed certs, inspecting certs, testing connections
8. Let's Encrypt and ACME protocol

### K. SSH

1. How SSH works: key exchange, authentication, encrypted channel
2. Password vs. key-based authentication
3. ssh-keygen: Ed25519, RSA, passphrase protection
4. ~/.ssh/config: host aliases, identity files, ProxyJump
5. ssh-agent: key caching, agent forwarding and security implications
6. Port forwarding: local (-L), remote (-R), dynamic/SOCKS (-D)
7. scp, sftp, rsync over SSH
8. SSH hardening: disable password auth, disable root login, fail2ban
9. Jump hosts/bastion hosts

### L. Firewalls and Network Security

1. Stateful vs. stateless firewalls
2. iptables: tables (filter, nat, mangle), chains (INPUT, OUTPUT, FORWARD), targets (ACCEPT, DROP, REJECT)
3. nftables: successor syntax
4. ufw (Ubuntu), firewalld (RHEL/Fedora)
5. Cloud security groups: AWS Security Groups, Azure NSGs, GCP Firewall Rules
6. Network ACLs

### M. Troubleshooting

1. Layer-by-layer methodology: L1 → L7
2. Tools by layer: ip link/ethtool (L1-2), ping/traceroute (L3), ss/nc (L4), curl/dig (L7)
3. tcpdump: -i, -n, -w, -r, BPF filter syntax
4. Wireshark: capture vs. display filters
5. Common issues: DNS failure, routing loops, MTU mismatch, firewall blocking, certificate errors

---

## Section 14. Databases

### A. Relational Model

1. Tables, rows, columns, keys (primary, foreign, candidate, composite)
2. Relational algebra: selection, projection, join, union, difference
3. Relational calculus: tuple and domain

### B. SQL

1. DDL: CREATE, ALTER, DROP, TRUNCATE
2. DML: SELECT, INSERT, UPDATE, DELETE
3. DCL: GRANT, REVOKE
4. TCL: BEGIN, COMMIT, ROLLBACK, SAVEPOINT

### C. Queries

1. SELECT: WHERE, ORDER BY, LIMIT, DISTINCT
2. JOINs: inner, left outer, right outer, full outer, cross, self
3. Subqueries: scalar, table, correlated
4. Aggregation: GROUP BY, HAVING, COUNT, SUM, AVG, MIN, MAX
5. Window functions: ROW_NUMBER, RANK, LAG, LEAD, OVER/PARTITION BY
6. CTEs: WITH clause, recursive CTEs
7. Set operations: UNION, INTERSECT, EXCEPT

### D. Data Modeling

1. Entity-relationship diagrams: entities, attributes, relationships, cardinality
2. Normalization: 1NF, 2NF, 3NF, BCNF — each with examples and rationale
3. Denormalization: when and why to break normal forms for performance
4. Schema design: one-to-one, one-to-many, many-to-many (junction tables)

### E. Indexing

1. B-tree indexes: structure, range queries, ordered access
2. Hash indexes: equality lookups, no range support
3. Composite indexes: column ordering, leftmost prefix rule
4. Covering indexes: including all queried columns
5. Index selectivity: cardinality and when indexes help vs. hurt
6. EXPLAIN/ANALYZE: reading query execution plans, identifying full table scans

### F. Views, Stored Procedures, Triggers, Functions

1. Views: virtual tables, materialized views
2. Stored procedures: server-side logic
3. Triggers: event-driven database actions
4. User-defined functions: scalar and table-valued

### G. Transactions and Concurrency

1. ACID: atomicity, consistency, isolation, durability
2. Transaction isolation levels: read uncommitted, read committed, repeatable read, serializable
3. Concurrency control: two-phase locking (2PL), timestamp ordering, MVCC
4. Deadlock detection and resolution in databases
5. Write-ahead logging (WAL)

### H. Practical Databases

1. PostgreSQL: features, psql CLI, pg_dump, extensions
2. MySQL/MariaDB: features, mysql CLI, mysqldump
3. SQLite: embedded, serverless, single-file
4. Database CLI tools and GUI clients

### I. NoSQL Overview

1. Key-value stores: Redis, DynamoDB/Cosmos DB/Bigtable concepts
2. Document stores: MongoDB, Firestore concepts
3. Column-family stores: Cassandra, HBase concepts
4. Graph databases: Neo4j concepts
5. CAP theorem: consistency, availability, partition tolerance
6. Eventual consistency vs. strong consistency
7. When to use relational vs. NoSQL

### J. Database Operations

1. Backups: logical (pg_dump) vs. physical (file-level), point-in-time recovery
2. Replication: primary-replica, synchronous vs. asynchronous
3. Connection pooling: PgBouncer, ProxySQL
4. Schema migrations: versioned migrations, expand-contract pattern, forward-only
5. Monitoring: slow query logs, pg_stat_statements

---

## Section 15. API Fundamentals

### A. What Is an API

1. API defined: a contract between software components
2. Why APIs exist: decoupling, reuse, abstraction, integration
3. API types: library/SDK, OS system calls, web APIs
4. Internal APIs, partner APIs, public APIs

### B. REST Architecture

1. REST constraints: client-server, stateless, cacheable, uniform interface, layered system
2. Resources as nouns: /users, /orders/{id}, /users/{id}/orders
3. HTTP methods mapped to CRUD: GET, POST, PUT, PATCH, DELETE
4. Idempotency: GET, PUT, DELETE are idempotent; POST is not
5. HATEOAS: what it means, why few APIs implement it
6. Richardson Maturity Model: Level 0 → Level 3

### C. Data Formats

1. JSON: syntax, nesting, common pitfalls
2. YAML: indentation-based, anchors/aliases, the Norway problem
3. JSON vs. YAML: APIs vs. configuration
4. XML: legacy, SOAP, enterprise
5. Protocol Buffers: binary serialization, schema, gRPC
6. jq: .key, .[0], | select(), | map(), @csv, -r
7. yq: YAML processing

### D. Making API Calls

1. curl: -X, -H, -d, -v, -i, -L, -s, --data-binary @file
2. Python requests: get, post, put, patch, delete, params, json, headers, response handling, sessions, timeouts
3. httpie: human-friendly alternative

### E. Authentication and Authorization

1. API keys: header-based, query parameter
2. Basic auth: Base64, requires HTTPS
3. Bearer tokens: Authorization header, JWT
4. JWT: header.payload.signature, claims, expiration, verification
5. OAuth 2.0: roles, flows (authorization code, client credentials, device code), scopes, tokens
6. Service accounts: non-human identities

### F. API Design Patterns

1. Versioning: URL path, header, query parameter — tradeoffs
2. Pagination: offset-based, cursor-based, keyset
3. Filtering and sorting: query parameters
4. Rate limiting: 429, X-RateLimit headers, backoff
5. Error responses: consistent format
6. Bulk operations: batch endpoints, async with polling
7. Idempotency keys

### G. Client Resilience

1. Retries: when to retry (5xx, timeouts) vs. not (4xx)
2. Exponential backoff with jitter
3. Circuit breaker pattern
4. Timeouts: connect vs. read, why defaults are dangerous

### H. Beyond REST

1. GraphQL: query language, single endpoint, schema, no over/under-fetching, N+1 problem
2. gRPC: Protocol Buffers, HTTP/2, streaming, performance
3. WebSockets: full-duplex, persistent connection, real-time
4. Webhooks: server-push events, callback registration, signature verification
5. Server-Sent Events (SSE): one-way streaming
6. When to use what

### I. API Documentation

1. OpenAPI (Swagger): YAML/JSON spec, paths, operations, schemas
2. Reading specs to construct valid requests
3. Swagger UI: interactive explorer
4. Postman/Insomnia: GUI testing tools
5. API-first design

---

## Section 16. Security Fundamentals

### A. Security Principles

1. CIA triad: confidentiality, integrity, availability
2. AAA: authentication, authorization, accounting
3. Least privilege: minimum permissions, blast radius reduction
4. Defense in depth: multiple layers
5. Zero trust: never trust, always verify, identity-based access
6. Threat modeling: STRIDE framework, assets, threats, vulnerabilities
7. Risk assessment: likelihood × impact

### B. Cryptography

1. Symmetric encryption: AES (128, 256), ChaCha20, modes (ECB, CBC, GCM), authenticated encryption
2. Asymmetric encryption: RSA, ECDSA, Ed25519, public/private key pairs
3. Hashing: SHA-256, SHA-3, BLAKE2, properties (deterministic, collision-resistant, avalanche)
4. Password hashing: bcrypt, scrypt, Argon2 — why SHA-256 is wrong for passwords
5. HMAC: integrity + authenticity
6. Digital signatures: non-repudiation
7. Key exchange: Diffie-Hellman, ECDHE
8. Randomness: /dev/urandom, CSPRNG

### C. PKI and Certificates

1. Certificate authorities: root, intermediate, trust stores
2. X.509: subject, issuer, serial, validity, SAN
3. Certificate chain validation
4. Certificate lifecycle: CSR, signing, distribution, renewal, revocation
5. Revocation: CRL, OCSP, OCSP stapling
6. Let's Encrypt and ACME
7. Self-signed certificates: when appropriate
8. openssl operations: generate, sign, inspect, verify

### D. Authentication

1. Factors: knowledge, possession, inherence
2. MFA: TOTP, FIDO2/WebAuthn, push notifications
3. Password security: length over complexity, managers, credential stuffing
4. SSO: SAML, OIDC, federation
5. OAuth 2.0 vs. OIDC: authorization vs. authentication
6. Service authentication: API keys, mTLS, workload identity

### E. Authorization

1. RBAC: roles, permissions, hierarchy
2. ABAC: attribute-based policies
3. ACLs: per-resource permissions
4. Policy engines: OPA, Cedar
5. Least privilege implementation: scoped tokens, temporary credentials, JIT access
6. Cloud IAM concepts: AWS IAM, Azure RBAC, GCP IAM

### F. Secrets Management

1. What counts as a secret
2. Why hardcoded secrets are critical vulnerabilities
3. Environment variables: better than hardcoded, still risky
4. Secret managers: Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager
5. Secret rotation: automated, zero-downtime patterns
6. Secret scanning: git-secrets, truffleHog, GitHub secret scanning
7. Vault seal/unseal: Shamir's secret sharing concept

### G. Common Vulnerabilities (OWASP Top 10)

1. Injection: SQL, command, LDAP — parameterized queries
2. Broken authentication: credential stuffing, session fixation
3. Sensitive data exposure: unencrypted at rest/in transit
4. XXE: XML parser exploits
5. Broken access control: IDOR, privilege escalation
6. Security misconfiguration: defaults, unnecessary features, missing patches
7. XSS: reflected, stored, DOM-based — output encoding, CSP
8. Insecure deserialization
9. Vulnerable dependencies: CVEs, SCA
10. Insufficient logging
11. SSRF: server-side request forgery

### H. System Hardening

1. Linux hardening: disable root SSH, key-based auth, firewall allow-list, remove unnecessary packages, automatic security updates, audit file permissions, auditd, SELinux/AppArmor enforcing, sysctl hardening
2. CIS Benchmarks: industry-standard guides
3. Patch management: vulnerability scanners (Nessus, OpenVAS, Trivy)

### I. Supply Chain Security

1. Software supply chain: source → build → dependencies → artifacts → deployment
2. Dependency scanning: npm audit, pip-audit, Dependabot, Snyk, Trivy
3. SBOM: CycloneDX, SPDX
4. Artifact signing: Sigstore, cosign, Notary
5. Reproducible builds, build provenance
6. SLSA framework: levels 1–4

### J. Incident Response

1. Phases: preparation → detection → containment → eradication → recovery → lessons learned
2. Detection: monitoring, alerting, SIEM
3. Containment and eradication
4. Recovery: backups, integrity verification
5. Post-incident: blameless post-mortems, action items
6. Communication: stakeholder notification, breach laws

### K. Compliance and Frameworks

1. SOC 2: Type I vs. Type II, trust service criteria
2. ISO 27001: ISMS, risk-based
3. PCI DSS: 12 requirements
4. HIPAA: PHI protection
5. GDPR: data protection, subject rights

---

## Section 17. Software Engineering

### A. Software Development Lifecycle

1. Requirements gathering and analysis
2. Waterfall model
3. Agile: Scrum, Kanban, XP
4. Spiral model and risk management
5. Iterative and incremental development
6. DevOps culture and practices

### B. Software Design

1. Modularity, coupling, cohesion
2. SOLID principles in depth (revisited with experience)
3. Design patterns (Gang of Four):
   a. Creational: Factory, Abstract Factory, Builder, Singleton, Prototype
   b. Structural: Adapter, Bridge, Composite, Decorator, Facade, Proxy
   c. Behavioral: Observer, Strategy, Command, Template Method, Iterator, State, Chain of Responsibility

### C. Architectural Patterns

1. MVC, MVP, MVVM
2. Layered architecture
3. Clean architecture, hexagonal architecture
4. Microservices vs. monolith: tradeoffs, boundaries, communication
5. Event-driven architecture
6. API design principles (revisited): REST, versioning, pagination, rate limiting

### D. Code Quality

1. Code smells: identification and classification
2. Refactoring techniques: extract method, rename, inline, move, introduce parameter object
3. Static analysis and linters
4. Technical debt: identification, measurement, management
5. Documentation: API docs, architecture decision records (ADRs)

### E. Build Systems

1. Build tools: Make, CMake concepts, Gradle, Maven, Webpack, Vite
2. Dependency management and package managers
3. Artifact generation and versioning

### F. Testing Strategy

1. Testing pyramid: unit (many, fast) → integration (fewer) → E2E (fewest, slow)
2. Test-driven development (TDD): red, green, refactor
3. Behavior-driven development (BDD): Given/When/Then
4. Test doubles: mocks, stubs, fakes, spies
5. Code coverage: statement, branch, path — what coverage misses
6. Property-based testing
7. Load testing and performance testing
8. Chaos engineering concepts

---

## Section 18. CI/CD

### A. CI/CD Concepts

1. Continuous Integration: merge frequently, build/test automatically
2. Continuous Delivery: every commit is releasable, deployment is manual
3. Continuous Deployment: every passing commit deploys automatically
4. Pipeline stages: source → build → test → scan → package → deploy → monitor
5. Trunk-based development and CI

### B. GitHub Actions

1. Workflow files: .github/workflows/*.yml
2. Triggers: push, pull_request, schedule, workflow_dispatch, release, workflow_call
3. Events and filters: branch, path, tag
4. Jobs: runners, needs, conditional if
5. Steps: uses (actions) vs. run (shell), env, with
6. Runners: GitHub-hosted, self-hosted
7. Matrix strategy: multi-version/multi-platform, include/exclude
8. Expressions and contexts: github, secrets, steps
9. Caching: actions/cache, dependency caching
10. Artifacts: upload/download between jobs
11. Secrets: repository, environment, organization, GITHUB_TOKEN
12. Environments: protection rules, reviewers, wait timers
13. Reusable workflows: workflow_call, inputs, secrets inheritance
14. Composite actions: action.yml
15. Concurrency groups

### C. Pipeline Design

1. Build stage: compile, bundle, generate
2. Test stage: testing pyramid in CI
3. Lint and format: fail on style violations
4. Security scanning: SAST, DAST, SCA, secret scanning
5. Container image build: tag with SHA and semver, push to registry
6. Artifact versioning: semantic versioning, immutable artifacts
7. Dependency caching, parallelism, fan-out/fan-in

### D. Deployment Strategies

1. Rolling deployment: gradual replacement
2. Blue-green: two environments, atomic switch
3. Canary: percentage-based traffic routing
4. Feature flags: deploy without activating
5. Database migrations in CI/CD: expand-contract
6. Rollback strategies
7. Smoke tests and health checks

### E. Quality Gates

1. Linting: ruff, eslint, shellcheck, hadolint
2. Formatting: black, prettier, gofmt
3. Test coverage thresholds
4. Security scanning: trivy, semgrep, gitleaks, checkov
5. License compliance
6. Required status checks and branch protection

### F. GitOps

1. Principles: declarative, Git as source of truth, automated reconciliation
2. GitOps workflow: push → detect drift → reconcile
3. Tools: Argo CD, Flux
4. Push vs. pull deployment: security implications
5. Environment promotion via Git
6. Drift detection and correction

### G. Other CI/CD Platforms

1. GitLab CI: .gitlab-ci.yml, stages, runners
2. Jenkins: Jenkinsfile, declarative vs. scripted, plugins
3. CircleCI: config.yml, orbs, workflows
4. Concept mapping across platforms
5. Cloud-native: AWS CodePipeline, Azure DevOps, GCP Cloud Build

### H. Pipeline Security

1. Least-privilege runners
2. OIDC authentication: CI tokens → cloud credentials
3. Signed artifacts: cosign, verification at deployment
4. Pin action versions by SHA
5. Pull request security: fork restrictions, pull_request_target risks
6. Secret hygiene: masking, rotation, per-environment separation

---

## Section 19. Containers

### A. Container Fundamentals

1. What a container is: isolated process(es) sharing the host kernel
2. Containers vs. VMs: no guest OS, shared kernel, faster, less isolation
3. Kernel features: namespaces (isolation), cgroups v2 (limits), OverlayFS (layers), seccomp (syscall filtering), capabilities
4. History: chroot → jails → zones → LXC → Docker → OCI

### B. OCI Specification

1. Image Spec: manifest, config, layers (tar + gzip/zstd), content-addressable
2. Runtime Spec: create, start, stop, delete from OCI bundles
3. Distribution Spec: push/pull from registries
4. Why standards matter: interoperability

### C. Container Runtimes

1. High-level: Docker Engine, Podman
2. Low-level: runc, crun, gVisor (runsc), Kata Containers
3. containerd: industry standard, used by Docker and Kubernetes
4. CRI-O: lightweight, Kubernetes CRI
5. Architecture: CLI → daemon → containerd → runc → container
6. Podman: daemonless, rootless, Docker-compatible
7. nerdctl: containerd CLI

### D. Building Images

1. Dockerfile: FROM, RUN, COPY, ADD, WORKDIR, ENV, ARG, EXPOSE, USER, ENTRYPOINT, CMD, LABEL, HEALTHCHECK
2. ENTRYPOINT vs. CMD: exec form vs. shell form, combination, overrides
3. Layer model: each instruction creates a layer, caching, order matters
4. Cache optimization: dependency files first, then application code
5. Multi-stage builds: separate build and runtime, smaller images
6. .dockerignore
7. Base image selection: full OS, slim, Alpine (musl), distroless, scratch
8. BuildKit: cache mounts, secret mounts, SSH mounts, multi-platform builds
9. Image security: non-root USER, minimize packages, pin versions, scan (trivy, grype), sign (cosign)

### E. Running Containers

1. docker run: -d, -it, --name, --rm, resource limits (--memory, --cpus)
2. Environment: -e, --env-file
3. Port mapping: -p host:container
4. Lifecycle: create → start → running → stop → remove, pause/unpause
5. docker exec, logs (-f, --tail), inspect (--format), cp, stats, top
6. Health checks: HEALTHCHECK, --health-cmd

### F. Networking

1. Bridge network: default, port mapping for external access
2. Host network: share host stack
3. None network: fully isolated
4. Custom bridge networks: automatic DNS by container name
5. Network drivers: bridge, host, overlay, macvlan, ipvlan
6. Published ports and IP binding

### G. Storage

1. Union/overlay filesystem: read-only layers + writable layer, copy-on-write
2. Volumes: managed, persistent, docker volume commands
3. Bind mounts: host directory mapping
4. tmpfs: RAM-backed, volatile
5. Volumes vs. bind mounts: tradeoffs
6. Data persistence patterns

### H. Docker Compose

1. compose.yaml: services, networks, volumes, configs, secrets
2. Service definition: image, build, ports, environment, volumes, depends_on, restart, healthcheck
3. depends_on conditions: service_healthy, service_started
4. Networking: default, custom, service name as DNS
5. Environment handling: .env, environment:, env_file:, substitution
6. Compose profiles
7. docker compose commands: up, down, logs, ps, exec, build
8. Compose watch for development
9. Override files

### I. Rootless Containers

1. Why rootless: defense in depth, reduced blast radius
2. Docker rootless mode
3. Podman rootless: default, no daemon
4. User namespace remapping: userns-remap, /etc/subuid, /etc/subgid
5. Limitations

### J. Container Registries

1. Public: Docker Hub, GHCR, Quay.io
2. Private: Harbor, ECR, ACR, Artifact Registry
3. Tags vs. digests: mutable pointer vs. immutable content address
4. Image naming: registry/namespace/repository:tag
5. Authentication: docker login, credential helpers, OIDC
6. Image scanning in registries
7. Multi-arch manifests

---

## Section 20. Container Orchestration

### A. Why Orchestration

1. Single-host limitations: scaling, HA, zero-downtime, service discovery
2. What orchestrators do: scheduling, scaling, networking, storage, self-healing
3. Landscape: Kubernetes, Docker Swarm, Nomad, ECS/Fargate
4. Kubernetes distributions: upstream, managed (EKS, AKS, GKE), lightweight (k3s, kind, minikube)
5. CNCF ecosystem

### B. Kubernetes Architecture

1. API server: REST API, admission controllers, authentication, authorization
2. etcd: distributed KV store, Raft consensus, source of truth
3. Scheduler: assigns Pods to nodes, filtering and scoring
4. Controller manager: Deployment, ReplicaSet, Node controllers
5. Cloud controller manager: load balancers, volumes, nodes
6. kubelet: node agent, Pod lifecycle
7. kube-proxy: iptables/IPVS, pod-to-service routing
8. Container runtime: containerd, CRI-O via CRI
9. Add-ons: CoreDNS, metrics-server, CNI
10. Declarative model: desired state → reconciliation → convergence

### C. Workload Resources

1. Pod: containers, initContainers, volumes, restartPolicy
2. Multi-container patterns: sidecar, ambassador, adapter
3. Pod lifecycle: Pending → Running → Succeeded/Failed
4. Probes: liveness, readiness, startup — HTTP/TCP/exec
5. ReplicaSet: replica count, label selectors
6. Deployment: rolling updates (maxSurge, maxUnavailable), Recreate, rollout commands
7. StatefulSet: stable identities, stable storage, headless Service, volume claim templates
8. DaemonSet: one Pod per node
9. Job: completions, parallelism, backoffLimit
10. CronJob: cron syntax, concurrencyPolicy

### D. Service Discovery and Networking

1. ClusterIP: internal, virtual IP
2. NodePort: static port on every node (30000–32767)
3. LoadBalancer: cloud LB provisioning
4. ExternalName: CNAME alias
5. Service mechanics: selectors, Endpoints/EndpointSlices
6. DNS: service.namespace.svc.cluster.local
7. Ingress: host/path routing, TLS termination
8. Ingress controllers: NGINX, Traefik, HAProxy, cloud-specific
9. Gateway API: HTTPRoute, GRPCRoute, Gateway classes
10. CNI: Calico, Cilium, Flannel, Weave
11. Pod-to-pod: flat network, every pod gets an IP
12. Network Policies: ingress/egress rules, namespace isolation, default deny
13. Service mesh: Istio, Linkerd — mTLS, traffic management, observability

### E. Configuration and Secrets

1. ConfigMaps: key-value, env vars, volume mounts, immutable
2. Secrets: base64, types (Opaque, tls, docker-registry), volume mounts
3. Encryption at rest: EncryptionConfiguration, KMS providers
4. Environment variables: env, envFrom, valueFrom, fieldRef, resourceFieldRef
5. External secret management: External Secrets Operator, Sealed Secrets, Vault

### F. Storage

1. Volumes: emptyDir, hostPath, configMap, secret, PVC, projected
2. PersistentVolume: capacity, access modes (RWO, ROX, RWX)
3. PersistentVolumeClaim: bound to PV
4. StorageClass: dynamic provisioning, reclaim policy, expansion
5. CSI: Container Storage Interface drivers
6. Volume snapshots

### G. RBAC and Security

1. Role and ClusterRole: verbs + resources + apiGroups
2. RoleBinding and ClusterRoleBinding: subjects (users, groups, ServiceAccounts)
3. ServiceAccounts: pod identity, automountServiceAccountToken
4. Pod Security Standards: Privileged, Baseline, Restricted
5. Pod Security Admission: namespace labels, enforce/audit/warn
6. Security contexts: runAsNonRoot, readOnlyRootFilesystem, capabilities, seccompProfile
7. Admission controllers: mutating, validating, webhooks
8. Policy engines: OPA Gatekeeper, Kyverno

### H. Resource Management

1. Requests: guaranteed, scheduling input
2. Limits: maximum, OOMKilled (memory), throttled (CPU)
3. LimitRange: per-namespace defaults and constraints
4. ResourceQuota: aggregate limits
5. QoS classes: Guaranteed, Burstable, BestEffort
6. HPA: scale on CPU, memory, custom metrics
7. VPA: adjust requests automatically
8. Pod Disruption Budgets

### I. Scheduling

1. Filtering and scoring: how the scheduler selects nodes
2. Node selectors: nodeSelector
3. Node affinity: required, preferred
4. Pod affinity and anti-affinity
5. Taints and tolerations: NoSchedule, PreferNoSchedule, NoExecute
6. Topology spread constraints
7. Priority and preemption: PriorityClasses

### J. Operators and CRDs

1. Custom Resource Definitions: extending the API
2. Custom controllers: watch, reconcile
3. Operator pattern: CRD + controller
4. Examples: database operators, cert-manager, Prometheus Operator
5. Operator frameworks: Operator SDK, Kubebuilder
6. Helm: charts, values, templates, releases, repositories

### K. Observability and Troubleshooting

1. kubectl essentials: get, describe, logs, exec, port-forward, apply, delete, edit, patch, explain, diff
2. Output formats: -o yaml/json/wide/jsonpath, --field-selector, -l
3. Debugging: Pod not starting (events, scheduler, image pull, resources), Pod crashing (logs, exit codes, probes, limits), Service unreachable (selectors, endpoints, network policies, DNS), PV not bound (StorageClass, access modes, capacity)
4. Events: kubectl get events --sort-by
5. Ephemeral debug containers: kubectl debug
6. Logging: stdout/stderr, Fluentd/Fluent Bit → Elasticsearch/Loki
7. Monitoring: Prometheus + Grafana, kube-state-metrics, node-exporter

---

## Section 21. Infrastructure as Code

### A. IaC Principles

1. What IaC solves: manual provisioning is slow, error-prone, undocumented
2. Declarative vs. imperative: "what" vs. "how"
3. Idempotency: same config applied multiple times → same result
4. Version control for infrastructure
5. Immutable infrastructure: replace instead of modify
6. The IaC landscape: Terraform/OpenTofu, Pulumi, CloudFormation, ARM/Bicep, CDK/CDKTF, Crossplane, Ansible

### B. Terraform Fundamentals

1. HCL: blocks, arguments, expressions, comments
2. Provider architecture: plugins translating HCL to API calls
3. Resources: resource "type" "name" { }, CRUD lifecycle, dependencies (implicit, depends_on)
4. Data sources: read existing infrastructure
5. Variables: type, default, description, validation, sensitive, input methods (-var, -var-file, TF_VAR_*, terraform.tfvars)
6. Outputs: value, description, sensitive
7. Locals: computed values, reducing repetition
8. Terraform block: required_version, required_providers, backend

### C. Plan/Apply Workflow

1. terraform init: providers, backend, modules
2. terraform validate: syntax check, no API calls
3. terraform plan: diff desired vs. actual, +/-/~ notation
4. terraform apply: execute changes
5. terraform destroy: remove all managed resources
6. -target, -auto-approve: when and when not to use
7. Refresh: drift detection from actual state

### D. State Management

1. What state is: JSON mapping resource addresses to real IDs
2. Local state: terraform.tfstate, limitations
3. Remote backends: S3+DynamoDB, Azure Storage, GCS, Terraform Cloud
4. State locking: prevent concurrent modification
5. State file format: resource instances, attributes, dependencies
6. State commands: list, show, mv, rm, pull, push
7. Drift detection: plan reveals differences
8. State security: contains sensitive values, encrypt at rest, restrict access

### E. HCL Expressions and Functions

1. Types: string, number, bool, list, map, set, object, tuple
2. String interpolation: "${var.name}"
3. Conditional: condition ? true_val : false_val
4. for expressions: [for s in var.list : upper(s)], filtering with if
5. Splat: resource[*].attribute
6. Functions: string (format, join, split, replace), collection (length, lookup, merge, flatten, keys, values, zipmap), filesystem (file, templatefile), encoding (jsonencode, yamlencode), IP (cidrsubnet, cidrhost), type (tostring, try, can)
7. Dynamic blocks: generate repeated nested blocks

### F. Resource Meta-Arguments

1. count: N copies, count.index, conditional (count = var.enabled ? 1 : 0)
2. for_each: from map/set, each.key, each.value
3. depends_on: explicit dependency
4. lifecycle: create_before_destroy, prevent_destroy, ignore_changes, replace_triggered_by
5. provider: aliases for multi-region
6. Provisioners: local-exec, remote-exec — last resort
7. moved block: refactor without destroy
8. import block (1.5+): declarative import

### G. Modules

1. What modules solve: reuse, encapsulation, composition
2. Structure: main.tf, variables.tf, outputs.tf, versions.tf
3. Sources: local, registry, Git, S3/GCS, HTTP
4. Inputs, outputs, versioning (version = "~> 3.0")
5. Root module vs. child modules
6. Composition: combining modules, passing outputs as inputs
7. Writing good modules: single responsibility, sensible defaults

### H. Multi-Environment Management

1. Workspaces: terraform workspace, terraform.workspace variable, limitations
2. Directory-based: environments/dev/, environments/prod/ with shared modules
3. Variable files: dev.tfvars, prod.tfvars
4. Terragrunt: DRY multi-environment management
5. Environment promotion: dev → staging → prod

### I. Testing and Policy

1. terraform validate, terraform fmt
2. terraform plan review: the most important quality gate
3. Terratest: Go-based integration testing
4. terraform test (1.6+): HCL-based tests, mock providers
5. OPA/conftest: Rego policy evaluation
6. Sentinel: enterprise policy framework
7. Checkov, tflint: static analysis and linting
8. Infracost: cost estimation

### J. IaC in CI/CD

1. Pipeline: lint → validate → plan → policy → approve → apply
2. Plan as PR comment: terraform show -json
3. Automated apply: apply saved plan after approval
4. OIDC authentication: no long-lived credentials
5. Terraform Cloud/HCP: managed runs, policy, state
6. Atlantis: PR automation
7. Drift detection: scheduled plans

### K. Secrets and Sensitive Data

1. sensitive = true: suppress in plan output
2. Sensitive values in state: protect the state file
3. Secret injection: CI/CD, Vault, OIDC
4. .tfvars with secrets: never commit
5. Data sources for secrets: Vault, cloud secret managers

### L. Advanced Topics

1. Provider development concepts: CRUD, schema, Plugin Framework
2. State internals: resource instances, deposed, dependency graph, serial
3. terraform graph: dependency visualization
4. terraform console: interactive expressions
5. Backend migration: init -migrate-state
6. Import workflow: import, generating config
7. Refactoring: moved blocks, state mv

---

## Section 22. Theory of Computation

### A. Automata Theory

1. Deterministic finite automata (DFA): definition, state diagrams, language recognition
2. Nondeterministic finite automata (NFA): epsilon transitions, nondeterminism
3. DFA-NFA equivalence: subset construction
4. Regular expressions and their equivalence to finite automata
5. Pumping lemma for regular languages
6. Pushdown automata (PDA): deterministic and nondeterministic, stack-based computation
7. Context-free grammars (CFG): production rules, derivations, parse trees
8. CFG-PDA equivalence
9. Pumping lemma for context-free languages
10. Chomsky normal form and Greibach normal form
11. Turing machines: definition, tape, head, transition function
12. Turing machine variants: multi-tape, nondeterministic, universal
13. Church-Turing thesis: what is computable
14. The Chomsky hierarchy: regular, context-free, context-sensitive, recursively enumerable

### B. Computability Theory

1. Decidable and undecidable languages
2. The halting problem: proof of undecidability by diagonalization
3. Rice's theorem: all non-trivial semantic properties of programs are undecidable
4. Reducibility: mapping reductions, Turing reductions
5. Recursively enumerable and co-recursively enumerable languages

### C. Complexity Theory

1. Time complexity classes: P, NP, co-NP
2. Polynomial-time reductions and NP-completeness
3. Cook-Levin theorem: SAT is NP-complete
4. Classic NP-complete problems: 3-SAT, Clique, Vertex Cover, Hamiltonian Cycle, TSP, Subset Sum
5. Approximation algorithms and heuristics (overview)

---

## Recommended Reading and Resources

### Foundational Texts

- Cormen, Thomas H., et al. *Introduction to Algorithms*. 4th ed., MIT Press, 2022.
- Sipser, Michael. *Introduction to the Theory of Computation*. 3rd ed., Cengage, 2013.
- Tanenbaum, Andrew S., and Herbert Bos. *Modern Operating Systems*. 4th ed., Pearson, 2015.
- Patterson, David A., and John L. Hennessy. *Computer Organization and Design*. 6th ed., Morgan Kaufmann, 2020.
- Kurose, James F., and Keith W. Ross. *Computer Networking: A Top-Down Approach*. 8th ed., Pearson, 2021.
- Silberschatz, Abraham, et al. *Database System Concepts*. 7th ed., McGraw-Hill, 2020.
- Gamma, Erich, et al. *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley, 1994.
- Rosen, Kenneth H. *Discrete Mathematics and Its Applications*. 8th ed., McGraw-Hill, 2019.
- Abelson, Harold, and Gerald Jay Sussman. *Structure and Interpretation of Computer Programs*. 2nd ed., MIT Press, 1996.

### Practical References

- Shotts, William. *The Linux Command Line*. 2nd ed., No Starch Press, 2019.
- Barrett, Daniel J., et al. *Linux Pocket Guide*. 4th ed., O'Reilly, 2024.
- Hightower, Kelsey, et al. *Kubernetes: Up and Running*. 3rd ed., O'Reilly, 2022.
- Brikman, Yevgeniy. *Terraform: Up and Running*. 3rd ed., O'Reilly, 2022.
- Kane, Sean P., and Karl Matthias. *Docker: Up and Running*. 3rd ed., O'Reilly, 2023.

### Online Resources

- MIT OpenCourseWare (ocw.mit.edu) — free CS course materials
- Stanford CS courses (online.stanford.edu) — algorithms, systems
- Khan Academy — mathematics foundations
- Linux man pages and official distribution documentation
- Kubernetes official documentation (kubernetes.io/docs)
- Terraform official documentation (developer.hashicorp.com/terraform)

---

## Assessment and Progression

Learners should demonstrate competency in each section before advancing. Assessment
methods vary by section type:

1. **Mathematical and theoretical sections** (2, 9, 10, 11, 22) — problem sets, written proofs, algorithm implementations
2. **Systems and tools sections** (3, 4, 5, 6, 7, 8, 12, 13, 14) — hands-on labs, build-and-break exercises, portfolio projects
3. **Infrastructure sections** (15, 16, 17, 18, 19, 20, 21) — end-to-end project deployments, architecture documentation, troubleshooting scenarios
4. **All sections** — the proficiency criteria defined in the detailed section outlines serve as the exit standard

---

*This unified syllabus merges core computer science theory with practical infrastructure
and automation skills into a single linear path. Upon completion, learners possess both
the theoretical foundation of a CS graduate and the operational proficiency of a cloud
infrastructure professional.*
