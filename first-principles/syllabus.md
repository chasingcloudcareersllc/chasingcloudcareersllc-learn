# Integrated Foundations Syllabus

## From Zero to Deep Expert — Theory and Practice as One

---

**Format:** MLA
**Subject:** Computer Science and Cloud Infrastructure — Integrated Curriculum
**Date:** 8 February 2026
**Scope:** Complete theoretical and practical foundation for deep expertise in computing,
from first principles through production infrastructure

---

## Course Description

This syllabus integrates core computer science theory with practical infrastructure
skills into a single path where neither exists without the other. Every theoretical
concept is immediately grounded in observable, hands-on practice. Every practical
skill is explained by the theory underneath it. Mathematics is embedded in the
domains that require it, not taught in isolation.

The result is a curriculum that produces professionals who can both *explain why*
and *show how* — who can reason from first principles when troubleshooting novel
problems, and who can operate production systems with the confidence that comes from
understanding what is happening beneath the surface.

### Design Principles

1. **No purely theoretical domains.** Every domain includes hands-on practice.
2. **No purely practical domains.** Every domain includes conceptual depth.
3. **Math is embedded, not front-loaded.** Mathematical concepts appear inside the
   domain that needs them, at the moment they become necessary.
4. **Topic-level integration.** Theory and practice alternate within individual topics,
   not just between sections. Learn the concept, observe it, apply it, connect it.
5. **Zero assumptions.** The learner has never touched a computer professionally.
6. **Provider-agnostic.** Concepts first, tools second. All major options referenced equally.

### What "Deep Expert" Means

A learner who completes this path can:

- **Explain internals** — not just use a tool, but describe how it works underneath
- **Troubleshoot from first principles** — diagnose problems others cannot, reasoning
  from foundational knowledge rather than pattern-matching error messages
- **Make architectural decisions** — choose between approaches with informed tradeoffs
- **Teach others** — explain concepts clearly enough to bring someone else up to speed

---

## Curriculum Overview

| # | Domain | Description |
|---|--------|-------------|
| 1 | Getting Started | Career landscape, environment setup, and learning strategy. |
| 2 | How Computers Work | Digital logic, processor architecture, hardware, memory hierarchy, data representation, and the boot process. |
| 3 | Operating Systems and Linux | Processes, scheduling, memory management, file systems, permissions, and system administration — theory and practice unified. |
| 4 | Text Processing and Automation | Vim, shell scripting, regular expressions, and the Unix text-processing philosophy. |
| 5 | Programming Fundamentals | Python, OOP, functional programming, testing, error handling, and project structure. |
| 6 | Data Structures and Algorithms | Formal data structures, complexity analysis, algorithm design paradigms — implemented and profiled. |
| 7 | Networking | Network models, protocols, routing, DNS, HTTP, TLS, SSH, firewalls, and packet-level troubleshooting. |
| 8 | Data Management | Relational model, SQL, normalization, transactions, indexing, NoSQL, and database operations. |
| 9 | Security and Cryptography | CIA triad, encryption, PKI, authentication, OWASP, hardening, secrets, and compliance. |
| 10 | APIs and Integration | REST, authentication, data formats, client resilience, GraphQL, gRPC, and webhooks. |
| 11 | Software Engineering and Collaboration | Version control, design patterns, architecture, CI/CD, testing strategy, and code quality. |
| 12 | Infrastructure at Scale | Containers, orchestration, infrastructure as code — the application of everything prior. |
| 13 | Theory of Computation | Automata, computability, and complexity — the formal limits of what machines can do. |

### Dependency Graph

```
1 Getting Started
└─▶ 2 How Computers Work
    └─▶ 3 Operating Systems and Linux
        ├─▶ 4 Text Processing and Automation
        │   └─▶ 5 Programming Fundamentals
        │       ├─▶ 6 Data Structures and Algorithms
        │       │   └─▶ 13 Theory of Computation
        │       └─▶ 11 Software Engineering and Collaboration
        │           └─▶ 12 Infrastructure at Scale
        └─▶ 7 Networking
            ├─▶ 8 Data Management
            ├─▶ 9 Security and Cryptography
            │   └─▶ 11 Software Engineering and Collaboration
            └─▶ 10 APIs and Integration
```

### Embedded Mathematics Map

No standalone math domains. Every mathematical concept is taught inside the domain
that needs it, at the moment it becomes necessary.

| Mathematical Topic | Taught In | Why It Is Needed There |
|--------------------|-----------|------------------------|
| Boolean algebra, De Morgan's laws | Domain 2: How Computers Work | Logic gate design, circuit simplification |
| Number systems (binary, hex, octal) | Domain 2: How Computers Work | Data representation, memory addresses, MAC addresses |
| Two's complement, IEEE 754 | Domain 2: How Computers Work | Integer and floating-point representation |
| Proof by induction | Domain 6: Data Structures and Algorithms | Proving algorithm correctness and termination |
| Recurrence relations, Master Theorem | Domain 6: Data Structures and Algorithms | Analyzing recursive algorithm complexity |
| Combinatorics (permutations, combinations) | Domain 6: Data Structures and Algorithms | Counting states in backtracking, combinatorial explosion |
| Probability (distributions, expected value) | Domain 6: Data Structures and Algorithms | Hash collision analysis, randomized algorithms, average-case complexity |
| Graph theory (formal) | Domain 6: Data Structures and Algorithms | BFS, DFS, shortest path, spanning trees, dependency graphs |
| Sets, relations, functions | Domain 6: Data Structures and Algorithms | Formal reasoning about types, equivalence, and correctness |
| Modular arithmetic | Domain 7 + 9: Networking, Security | IP subnetting, RSA, Diffie-Hellman |
| Number theory (primes, GCD, Euler's theorem) | Domain 9: Security and Cryptography | RSA key generation, cryptographic foundations |
| Bayes' theorem | Domain 9: Security and Cryptography | False positive reasoning in anomaly detection |
| Finite automata, regular languages | Domain 13: Theory of Computation | Formalizing regex (used since Domain 4), parsing, language recognition |

---

## Domain 1. Getting Started

*Prerequisites: None*

### Learning Objectives

1. You can describe what cloud computing, DevOps, SRE, and platform engineering roles do day-to-day and where they overlap.
2. You can set up a local learning environment: terminal emulator, Linux VM or cloud instance, SSH access.
3. You can navigate official documentation for any technology and extract what you need.
4. You can articulate your own learning plan with concrete milestones.

### Topic Outline

#### A. The Tech Career Landscape

1. What cloud computing is and why it exists
2. Role taxonomy: DevOps, SRE, cloud engineer, platform engineer, infrastructure engineer
3. What these roles share vs. where they diverge
4. Day-in-the-life breakdowns
5. The job market: what employers screen for, how theory and practice both matter

#### B. How This Path Works

1. The 13-domain structure: why theory and practice are integrated, not separated
2. How math is embedded — you learn it when you need it
3. The three assessment types: explain, build, debug
4. Effort benchmarks (not time estimates)

#### C. Setting Up Your Environment

1. Choosing a terminal emulator (Terminal.app, Windows Terminal, GNOME Terminal, Alacritty, iTerm2)
2. Installing a hypervisor (VirtualBox, UTM, QEMU/KVM, Hyper-V) or provisioning a cloud instance
3. Choosing a Linux distribution (Ubuntu, Fedora, Arch — tradeoffs, no single right answer)
4. Verifying your setup: SSH in, run basic commands

#### D. Learning Strategy

1. Active recall vs. passive review
2. Spaced repetition for command and concept retention
3. The build-break-fix loop: learning by destroying things intentionally
4. Lab notebooks: documenting what you learn in plain text
5. Reading documentation: man pages, --help, official docs, RFCs

#### E. Certifications and Credentials

1. When certifications help (career switching, HR filters, structured learning)
2. When they do not (credential-stacking without depth)
3. Major paths: AWS/Azure/GCP, CKA/CKAD, Terraform Associate, CompTIA, RHCSA
4. How this path maps to certification preparation

### Proficiency Criteria

You have a working Linux environment you built yourself, a written learning plan, and you treat official documentation as your first stop. You can explain the career landscape without buzzwords.

---

## Domain 2. How Computers Work

*Prerequisites: Domain 1*

### Learning Objectives

1. You can explain every major hardware component and how they interact.
2. You can trace the CPU instruction cycle and explain pipelining, branch prediction, and out-of-order execution.
3. You can simplify Boolean expressions using algebraic laws and map them to logic circuits.
4. You can design simple combinational and sequential circuits from logic gates.
5. You can convert between binary, decimal, and hexadecimal and explain integer and floating-point representation.
6. You can describe the memory hierarchy with latency numbers at each layer and explain why it exists.
7. You can trace the boot process from power-on through login prompt.
8. You can explain virtualization at the hardware level and differentiate hypervisor types.
9. You can inspect and interpret hardware information on a running Linux system using command-line tools.

### Topic Outline

*Each topic integrates the relevant theory and the practical observation of that theory on a real system.*

#### A. Boolean Algebra and Digital Logic

**Math (embedded):** Boolean algebra — AND, OR, NOT, XOR, De Morgan's laws, distributive, associative, commutative, identity, complement. Karnaugh maps for simplification.

**Theory:** Logic gates (AND, OR, NOT, NAND, NOR, XOR, XNOR). Combinational circuits: multiplexers, demultiplexers, encoders, decoders, half-adders, full-adders, ripple-carry adders. Sequential circuits: SR latch, D flip-flop, JK flip-flop, T flip-flop. Registers and counters. Finite state machines: Mealy and Moore models, state diagrams, state tables. Programmable logic: PLDs, FPGAs.

**Connection:** Boolean algebra is not abstract math here — it is the language of circuit design. Simplify an expression, draw the gate diagram, understand that this is what hardware *is*.

#### B. Processor Architecture

**Theory:** Instruction set architecture (CISC vs. RISC). Assembly language concepts (x86, ARM). Instruction encoding and addressing modes. The fetch-decode-execute cycle. Single-cycle, multi-cycle, and pipelined datapaths. Pipeline hazards (structural, data, control). Hazard mitigation: forwarding, stalling, branch prediction. Superscalar and out-of-order execution. Speculative execution.

**Practice:** Run `lscpu` and interpret every field: architecture, cores, threads, cache sizes, flags (including virtualization extensions). Compare output across different machines (VM vs. bare metal, x86 vs. ARM).

**Connection:** The `lscpu` flags are not random strings — each one corresponds to a hardware capability you just learned about. `vmx` or `svm` means virtualization extensions are present. Cache sizes map directly to the memory hierarchy.

#### C. Hardware Components

**Theory:** CPU (cores, threads, clock speed, microarchitecture, VT-x/AMD-V). RAM (DRAM vs. SRAM, DDR generations, channels, ECC). Storage (HDD platters/seek time vs. SSD NAND/wear leveling/TRIM, NVMe vs. SATA). GPU (parallel processing, CUDA/OpenCL). NIC (speeds, offloading, SR-IOV). Motherboard (chipset, PCIe lanes, form factors). PSU (wattage, efficiency).

**Practice:** Identify every component on your machine: `lscpu` (CPU), `free -h` (RAM), `lsblk` and `smartctl` (storage), `lspci` (PCI devices including GPU and NIC), `dmidecode` (motherboard, BIOS, memory layout), `lsusb` (peripherals).

**Connection:** A spec sheet is not marketing material — it is a description of the machine's capabilities and bottlenecks that you can now read and evaluate.

#### D. Memory Hierarchy

**Theory:** Registers (sub-ns) → L1 (1ns) → L2 (3–5ns) → L3 (10–15ns) → RAM (50–100ns) → SSD (10–100μs) → HDD (1–10ms). Why the hierarchy exists: physics forces a speed-capacity tradeoff. Cache organization: direct-mapped, set-associative, fully associative. Cache policies: write-through, write-back, write-allocate. Cache replacement: LRU, FIFO, random. Cache coherence: MESI protocol, cache lines, false sharing. NUMA: non-uniform memory access, multi-socket implications.

**Practice:** Read cache sizes from `lscpu`. Observe memory layout with `dmidecode --type memory`. Check NUMA topology with `numactl --hardware` (if applicable). Examine `/proc/meminfo` for detailed memory breakdown.

**Connection:** When a program is slow, the bottleneck is often in the memory hierarchy. Understanding cache behavior and NUMA locality explains performance patterns that are invisible without this knowledge.

#### E. Data Representation

**Math (embedded):** Number systems — binary, octal, decimal, hexadecimal. Conversion between bases. Why base-2 is native to digital circuits.

**Theory:** Bits, bytes, words. Integer representation: unsigned, signed (two's complement), overflow. Floating-point: IEEE 754, why 0.1 + 0.2 != 0.3. Character encoding: ASCII, Unicode, UTF-8, UTF-16. Endianness: big-endian vs. little-endian, network byte order. Error detection and correction: parity, checksums, Hamming codes, CRC. Units: bits vs. bytes, KB vs. KiB.

**Practice:** Convert numbers by hand, verify with `printf '%x\n' 255` and `python3 -c "print(0.1 + 0.2)"`. Inspect file encoding with `file` and `xxd`. Observe byte order with `lscpu | grep "Byte Order"`.

**Connection:** Encoding is not trivia — it is the reason files display garbled characters, checksums fail, and floating-point arithmetic surprises you. Understanding representation prevents entire categories of bugs.

#### F. Software Layers and the Boot Process

**Theory:** Firmware (BIOS, UEFI, Secure Boot, TPM). Partition tables (MBR vs. GPT). Bootloader (GRUB2). Kernel loading (initramfs, kernel parameters, modules). Init system (SysVinit → systemd, PID 1). Login (getty, PAM, display manager vs. TTY). Kernel space vs. user space. System calls as the boundary.

**Practice:** Trace the boot process on your VM by reading `dmesg` output. Identify each phase: hardware detection, driver loading, filesystem mount, systemd targets. Examine the partition table with `fdisk -l` or `gdisk -l`. Check whether your system uses BIOS or UEFI (`ls /sys/firmware/efi`). Inspect bootloader config (`/boot/grub/grub.cfg`).

**Connection:** When a system fails to boot, you are debugging this exact sequence. Knowing which phase failed (POST? bootloader? kernel? init?) determines where you look.

#### G. Virtualization and Emulation

**Theory:** Type 1 hypervisors (ESXi, Hyper-V, Xen, KVM). Type 2 hypervisors (VirtualBox, VMware, Parallels, UTM). Hardware-assisted virtualization (ring -1, VMCS, VMEXIT/VMENTER). Paravirtualization (virtio drivers). Emulation vs. virtualization (QEMU modes). Containers vs. VMs (preview — detailed in Domain 12).

**Practice:** Verify VT-x/AMD-V support: `grep -E 'vmx|svm' /proc/cpuinfo`. Compare `lscpu` output on bare metal vs. inside a VM. If using KVM, inspect with `virsh`.

### Exercises

- Simplify a Boolean expression, draw the gate circuit, verify with a truth table
- Identify every hardware component on your machine using only command-line tools and explain what each does
- Convert a set of numbers between binary, decimal, and hex by hand — then verify programmatically
- Read `dmesg` output and annotate each phase of the boot process
- Explain the memory hierarchy (with latency numbers) to a rubber duck, then verify cache sizes on your machine

### Proficiency Criteria

You can draw the boot sequence from power-on to login without notes. You can simplify Boolean expressions and map them to circuits. You can read a hardware spec sheet and identify bottlenecks. You can explain the memory hierarchy, cache coherence, and virtualization extensions to someone who has never heard of them. You can inspect all of this on a running system.

---

## Domain 3. Operating Systems and Linux

*Prerequisites: Domain 2*

### Learning Objectives

1. You can explain the kernel's core responsibilities and the boundary between kernel space and user space.
2. You can describe the process lifecycle (fork, exec, states, termination) and observe each state on a running system.
3. You can explain scheduling algorithms (CFS, round-robin, priority) and demonstrate their effects with real processes.
4. You can explain synchronization primitives (mutex, semaphore) and the four conditions for deadlock.
5. You can describe virtual memory, paging, page replacement algorithms, and observe memory behavior through /proc.
6. You can explain file system internals (inodes, journaling, VFS) and manage disks, partitions, and mount points.
7. You can explain I/O techniques (programmed, interrupt-driven, DMA) and disk scheduling algorithms.
8. You can manage users, groups, permissions (DAC, ACLs, capabilities), and sudo configuration.
9. You can manage packages across distributions (apt, dnf, pacman) and write systemd service units.
10. You can configure and troubleshoot networking from the command line.
11. You can profile system performance and tune kernel parameters via /proc, /sys, and sysctl.
12. You can explain namespaces and cgroups and demonstrate manual container-like isolation.

### Topic Outline

#### A. What an Operating System Does

**Theory:** Abstraction, resource management, isolation. Kernel architectures: monolithic (Linux), microkernel (Minix, QNX), hybrid (Windows NT, macOS/XNU). System calls: the API between user space and kernel space. Common syscalls: open, read, write, close, fork, exec, mmap, ioctl.

**Practice:** Use `strace ls` to trace system calls. Identify which syscalls correspond to which operations. Count syscalls with `strace -c`.

**Connection:** Every command you run on Linux is a sequence of system calls. `strace` makes the invisible boundary between user space and kernel space visible.

#### B. Processes

**Theory:** Process: PID, PPID, address space, file descriptors, environment, PCB. `fork()`: creating a child (copy-on-write). `exec()`: replacing the process image. `fork()+exec()`: how shells launch programs. Process states: running, sleeping (interruptible/uninterruptible), stopped, zombie. Zombie and orphan processes. Context switching cost.

**Practice:** `ps aux` to list all processes. `top`/`htop` for interactive monitoring. Create a zombie process intentionally (fork without wait), observe with `ps`, clean it up. Use `pstree` to see the process tree. Inspect `/proc/[pid]/status`, `/proc/[pid]/maps`, `/proc/[pid]/fd`.

**Connection:** A zombie is not a bug to Google — it is a child process that exited before its parent called `wait()`. Once you understand the lifecycle, the fix is obvious.

#### C. Threads and Synchronization

**Theory:** Threads: shared address space, pthreads, kernel vs. user threads, 1:1 and M:N models. Race conditions and critical sections. Synchronization primitives: mutexes, semaphores, monitors, condition variables. Classic problems: producer-consumer, readers-writers, dining philosophers. Spinlocks vs. blocking locks.

**Practice:** Write a Python script using `threading` that demonstrates a race condition (incrementing a shared counter without a lock). Add a lock, observe correct behavior. Use `time` to measure the GIL's impact.

**Connection:** Thread safety is not an academic concern — it is why database transactions exist, why web servers use worker pools, and why Kubernetes controllers use leader election.

#### D. Deadlock

**Theory:** Four conditions: mutual exclusion, hold and wait, no preemption, circular wait. Detection: wait-for graphs, cycle detection. Prevention: break one condition. Avoidance: Banker's algorithm. Recovery: process termination, resource preemption.

**Practice:** Create a deadlock with two shell scripts that each hold a `flock` while waiting for the other's. Observe the hung processes, identify the circular wait, kill to resolve.

**Connection:** Deadlocks appear in databases (transaction locks), distributed systems (resource contention), and even Terraform (state file locking). Recognizing the four conditions is how you diagnose them everywhere.

#### E. CPU Scheduling

**Theory:** Preemptive vs. cooperative. Algorithms: FCFS, SJF, SRTF, round-robin, priority, multilevel feedback queue. Linux CFS: virtual runtime, red-black tree, nice values, weight. Real-time scheduling: SCHED_FIFO, SCHED_RR, SCHED_DEADLINE. Load average: what the three numbers mean.

**Practice:** Run two CPU-bound processes. Use `nice` and `renice` to change priorities, observe CPU allocation shift in `top`. Check scheduling policy with `chrt -p [pid]`. Set CPU affinity with `taskset`. Read `/proc/[pid]/sched` for scheduler statistics.

**Connection:** When a container is "CPU-throttled" in Kubernetes, it is because cgroups are limiting CFS shares. Understanding the scheduler tells you whether the problem is starvation or genuine overload.

#### F. Memory Management

**Theory:** Physical vs. virtual memory. Paging: page size (4KB, huge pages 2MB/1GB), page tables (single-level, multi-level, inverted). TLB. MMU. Page faults: minor vs. major. Page replacement algorithms: FIFO, LRU, optimal, clock. Demand paging. Thrashing and working set model. Swap: partitions, files, zswap, zram. OOM killer: trigger, scoring, oom_score_adj. Memory overcommit settings. Memory-mapped files (mmap). NUMA awareness.

**Practice:** Read `/proc/meminfo` — map each field to the theory. Watch page faults with `perf stat -e page-faults ./program`. Trigger the OOM killer by allocating memory in a cgroup-limited process. Adjust swappiness via `sysctl vm.swappiness`. Configure huge pages and verify with `/proc/meminfo | grep Huge`.

**Connection:** When a Kubernetes pod is OOMKilled, the OOM killer scored it and terminated it. Understanding memory accounting, overcommit, and cgroup limits turns a cryptic kill into a debuggable event.

#### G. File Systems

**Theory:** Inodes: metadata, inode table, exhaustion. Dentries and the dcache. Superblock. File system implementation: allocation methods (contiguous, linked, indexed). Free space management (bitmaps, linked lists). Journaling: write-ahead logging, modes (writeback, ordered, journal). Types: ext4, XFS, Btrfs, ZFS, NTFS, tmpfs, procfs, sysfs. VFS layer. Hard links vs. symlinks. File descriptors.

**Practice:** `stat` a file — map every field to inode metadata. Check inode usage with `df -i`. Create hard and symbolic links, delete the original, observe what survives and why. Inspect file descriptors with `ls -la /proc/self/fd`. Create and mount a filesystem: `fdisk` → `mkfs.ext4` → `mount` → `/etc/fstab` with UUID (`blkid`). Manage LVM: create PV, VG, LV, resize. Configure RAID with `mdadm`.

**Connection:** "No space left on device" with disk space available means inode exhaustion. You know this because you understand that files are inodes, not just names on disk.

#### H. I/O Systems

**Theory:** I/O hardware: ports, buses, controllers. I/O techniques: programmed I/O, interrupt-driven, DMA. Device drivers. Disk scheduling: FCFS, SSTF, SCAN, C-SCAN, LOOK. I/O buffering and caching.

**Practice:** Monitor disk I/O with `iostat`, `iotop`. Read SMART data with `smartctl`. Benchmark with `hdparm` or `fio`. Observe I/O scheduler: `cat /sys/block/sda/queue/scheduler`.

#### I. Permissions and Access Control

**Theory:** Unix DAC: owner, group, others. Numeric vs. symbolic. umask. Special permissions: setuid, setgid, sticky bit. ACLs: getfacl, setfacl. Linux capabilities (CAP_NET_BIND_SERVICE, CAP_SYS_ADMIN). SELinux and AppArmor: MAC overview, enforcing vs. permissive.

**Practice:** Create a user, set permissions, test access. Configure sudo with specific command restrictions via `visudo`. Set ACLs on a directory. Use `getcap`/`setcap` to grant a binary a specific capability without root.

#### J. User and Package Management

**Practice (grounded in prior theory):** `/etc/passwd`, `/etc/shadow`, `/etc/group` formats. `useradd`, `usermod`, `userdel`, `passwd`. PAM modules. Package management: apt (Debian/Ubuntu), dnf (Fedora/RHEL), pacman (Arch) — install, remove, search, query across all three. Package formats: .deb, .rpm, .pkg.tar.zst. Low-level: dpkg, rpm. Universal: Snap, Flatpak, AppImage. Building from source.

#### K. systemd

**Practice (grounded in prior theory about init and PID 1):** Unit files: service, timer, socket, mount, target. `systemctl` operations. Writing custom service units: [Unit], [Service], [Install], ExecStart, Restart, WantedBy. Service types: simple, forking, oneshot, notify. Targets as runlevel equivalents. Timers: monotonic vs. realtime, OnCalendar. `journalctl`: -u, -f, --since, -p. `systemd-analyze`: boot time, blame, critical-chain.

#### L. Networking on Linux

**Practice (deep networking theory comes in Domain 7):** `ip addr`, `ip route`, `ip link`. `ss`. `nmcli`/`nmtui`. `/etc/hosts`, `/etc/resolv.conf`. `hostnamectl`. `ping`, `traceroute`, `mtr`. `dig`, `curl`, `wget`. Firewall basics: `iptables`, `nftables`, `ufw`/`firewalld`.

#### M. Performance Analysis

**Practice:** `vmstat`, `iostat`, `sar`, `mpstat`, `pidstat`. `perf` basics: stat, top, record, report. `/proc` deep dive: cpuinfo, meminfo, loadavg, [pid]/status, [pid]/maps. `/sys` deep dive: class, block, fs/cgroup. Kernel tuning: `sysctl`, `/etc/sysctl.conf`, common parameters (net.core.somaxconn, vm.swappiness, fs.file-max).

#### N. Namespaces and Cgroups

**Theory:** Namespaces: PID, network, mount, UTS, IPC, user, cgroup — what each isolates. cgroups v2: unified hierarchy, controllers (cpu, memory, io, pids). Resource limits. seccomp: syscall filtering.

**Practice:** Use `unshare` to create a new PID namespace — observe PID 1 inside is your shell. Use `nsenter` to enter an existing namespace. Create a cgroup, add a process, limit it to 256MB and 50% CPU, verify the limits work. This is what Docker does — you just did it by hand.

#### O. Environment and Shell Configuration

**Practice:** Environment variables: export, PATH, HOME, SHELL. Shell startup files: login vs. non-login, interactive vs. non-interactive. `source`. Shell options: set -e, -u, shopt. Aliases and functions. XDG Base Directory Specification.

### Exercises

- Trace a command with `strace`, identify every syscall, explain what each does
- Create a zombie process, observe it, explain why it exists, clean it up
- Create a deadlock between two processes using `flock`, identify the circular wait, resolve it
- Set up cgroups to limit a process to 50% CPU and 256MB RAM, run a memory hog, observe the OOM kill, explain the scoring
- Use `unshare` to create PID + mount namespace isolation, then explain: "I just built a container by hand"
- Build a pipeline: partition a disk (GPT), create ext4, mount via fstab with UUID, create LVM on top, resize it
- Write a custom systemd service, configure restart-on-failure, tail logs with `journalctl -f`
- Profile a CPU-bound process with `perf stat`, identify bottleneck, explain cache miss implications

### Proficiency Criteria

You can explain the process lifecycle from `fork()` to `exit()` including every state — and demonstrate each state on a running system. You can describe how CFS decides what runs next and prove it with `nice`/`renice`. You can explain page replacement algorithms and observe page faults with `perf`. You can set up namespace and cgroup isolation by hand. You manage users, packages, services, disks, and networking entirely from the terminal on any major distribution. You do not separate "OS theory" from "Linux administration" — they are the same thing.

---

## Domain 4. Text Processing and Automation

*Prerequisites: Domain 3*

### Learning Objectives

1. You can edit files in Vim fluently using modal editing, motions, operators, text objects, registers, and macros.
2. You can write Bash scripts with argument parsing, error handling, control flow, and signal trapping.
3. You can build multi-stage text processing pipelines with grep, sed, awk, sort, and pipes.
4. You can use regular expressions for pattern matching and explain their connection to finite automata (formalized in Domain 13).
5. You can schedule tasks with cron and systemd timers, and prevent concurrent execution with flock.
6. You can debug scripts systematically and know when to switch to Python.

### Topic Outline

#### A. Vim

**Theory (embedded):** Vim's modes are a finite state machine — normal, insert, visual, command-line are states, keypresses are transitions. This is not a metaphor. The modal model is efficient because most editing time is spent navigating and thinking, not typing.

**Practice:** Modal model. Navigation: h/j/k/l, w/b/e, 0/^/$, gg/G, /search, marks. Operators and text objects: d, c, y + iw, aw, i", a(, ip — composable grammar. The dot command. Registers: unnamed, named, system clipboard, black hole, expression. Search and replace: :%s with regex, :g/pattern/command, :v. Macros: record, play, recursive. Buffers, windows, tabs. Configuration: .vimrc settings, mappings, autocommands. Neovim: init.lua, LSP.

**Connection:** The operator + text object composability (`ciw` = change inner word, `da(` = delete around parens) is a *language*, not a set of shortcuts. Once you internalize the grammar, you can construct commands you have never seen before.

#### B. Text Processing Pipeline

**Practice:** grep (basic/extended regex, -r, -l, -c, -v, -i). sed (substitution, deletion, in-place, address ranges). awk (field processing, $1, $NF, BEGIN/END, pattern matching). sort, uniq (-c), cut, tr, wc, diff, tee. Multi-stage pipelines: extract → filter → transform → aggregate.

**Theory (embedded — regular expressions):** Regex syntax: character classes, quantifiers (*, +, ?, {n,m}), anchors (^, $), groups, backreferences, alternation. The connection to automata: a regex defines a pattern that a finite automaton can recognize. This is formalized in Domain 13, but the practical intuition starts here.

**Connection:** When `grep -E '^\d{1,3}(\.\d{1,3}){3}$'` matches an IP address, it is because the regex describes a regular language that a DFA can accept. You do not need to know the formal theory yet — but you will, and when you do, this will click.

#### C. Shell Scripting

**Practice:** Shebang (#!/usr/bin/env bash). Exit codes ($?, convention). Variables: assignment, quoting (single/double/none — word splitting and globbing), parameter expansion (${VAR:-default}, ${VAR#pattern}, ${VAR/old/new}). Arrays, associative arrays. Arithmetic ($(( ))). Special variables ($0, $@, $#, $$, $!).

Conditionals: test / [ ] / [[ ]], string/integer/file tests, case statements. Loops: for-in, C-style for, while read -r, until, select, safe file iteration (nullglob, spaces). I/O: redirection (>, >>, 2>, &>, 2>&1), here documents, here strings, pipes (PIPESTATUS, pipefail), process substitution (<(cmd)), tee, named pipes (mkfifo), exec for FD management.

Functions: declaration, arguments ($1–$9, $@), return values (exit codes + stdout capture), local variables, sourcing libraries. Error handling: set -euo pipefail (the defensive preamble), trap (EXIT, ERR, INT, TERM), error messages to stderr. Debugging: set -x, bash -x, PS4 with file and line.

Scheduling: cron (syntax, crontab -e/-l, environment pitfalls, timezone issues), systemd timers (OnCalendar, advantages over cron), at, flock for mutual exclusion.

Practical patterns: getopts, mktemp + trap cleanup, jq for JSON (.key, .[0], select, map, @csv), POSIX vs. Bash portability.

**Tool:** shellcheck — static analysis. Every script passes shellcheck before it is considered done.

### Exercises

- Complete vimtutor in under 20 minutes, then edit a real file using only motions and operators (no arrow keys)
- Use a Vim macro to convert 50 lines of CSV into JSON
- Build a log analysis pipeline: extract IPs with grep, count with sort | uniq -c, sort by frequency, output top 10 as CSV
- Write a script with getopts (-v for verbose, -o FILE), input validation, trap EXIT cleanup, and proper error handling that passes shellcheck
- Create a cron job and equivalent systemd timer, compare logging behavior
- Write a reusable function library (logging, temp file management, argument validation), source it from two scripts

### Proficiency Criteria

You edit in Vim at the speed of thought — motions and operators compose naturally, macros are routine, and the dot command is instinctive. Your scripts start with `set -euo pipefail`, clean up with `trap EXIT`, validate input, and pass shellcheck. You can build any text processing pipeline from grep, sed, awk, and pipes. You know when shell is the right tool and when to reach for Python.

---

## Domain 5. Programming Fundamentals

*Prerequisites: Domain 4*

### Learning Objectives

1. You can write Python programs with correct syntax, idiomatic style (PEP 8), and type hints.
2. You can use all built-in data structures (lists, tuples, dicts, sets) and choose correctly based on access patterns and complexity.
3. You can write functions with *args, **kwargs, decorators, closures, and explain scope (LEGB).
4. You can implement classes with inheritance, dunder methods, properties, and explain SOLID principles.
5. You can explain pure functions, immutability, map/filter/reduce, and apply functional patterns.
6. You can handle errors with try/except/else/finally, context managers, and custom exceptions.
7. You can read/write files and parse JSON, YAML, CSV programmatically.
8. You can manage virtual environments and dependencies (venv, pip, uv, requirements.txt, pyproject.toml).
9. You can write and run tests with pytest (fixtures, parametrize, mocking, coverage).
10. You can use the standard library effectively (pathlib, subprocess, argparse, logging, re, datetime, collections).
11. You can explain Python's execution model (bytecode, GIL, reference counting) and its implications.

### Topic Outline

#### A. Python Core

Syntax, variables, dynamic typing, numbers (arbitrary precision int, IEEE 754 float), strings (f-strings, slicing, immutability), booleans (truthy/falsy, short-circuit), None (is None). PEP 8, black, ruff.

#### B. Built-in Data Structures

Lists (ordered, mutable, O(1) append, O(n) insert). Tuples (immutable, namedtuple). Dicts (hash table underneath — O(1) average lookup, .get(), .setdefault(), comprehensions, defaultdict, Counter). Sets (O(1) membership, union/intersection/difference). Deque, heapq. Choosing the right structure: access patterns, ordering, mutability.

**Connection to Domain 6:** You use these data structures now. In Domain 6, you implement them from scratch and analyze their complexity formally.

#### C. Control Flow

if/elif/else, ternary. for (range, enumerate, zip), while, break/continue, else clause. Match statement. Iterators and generators: __iter__/__next__, yield, generator expressions, lazy evaluation. itertools: chain, islice, groupby, product, permutations.

#### D. Functions

Parameters: positional, keyword, default, *args, **kwargs, keyword-only, positional-only. Type hints (typing module). Docstrings. First-class functions, lambda, closures (nonlocal). Decorators: @decorator, functools.wraps, @staticmethod, @classmethod, @property. Scope: LEGB rule.

#### E. Object-Oriented Programming

Classes, __init__, self. Instance vs. class attributes. Dunder methods: __repr__, __str__, __eq__, __hash__, __len__, __getitem__, __iter__, __enter__/__exit__. Properties. Inheritance: single, multiple, MRO, super(). Abstract classes (abc.ABC). Dataclasses. Protocols (typing.Protocol). Composition vs. inheritance. SOLID principles.

#### F. Functional Programming

Pure functions and side effects. Immutability. Higher-order functions. Map, filter, reduce. Pattern matching (match/case). When FP patterns improve clarity vs. when they obscure it.

#### G. Paradigm Awareness

Imperative/procedural, OOP, functional, declarative, event-driven, concurrent (threading, multiprocessing, async/await preview). No paradigm is universally best — understand the tradeoffs.

#### H. Error Handling

try/except/else/finally. Built-in exceptions and hierarchy. Custom exceptions. EAFP vs. LBYL. Context managers: with, __enter__/__exit__, contextlib.contextmanager. Exception chaining.

#### I. File I/O and Data Formats

pathlib (Path objects, /, .exists(), .glob(), .read_text()). JSON: json.loads/dumps/load/dump. YAML: pyyaml, yaml.safe_load. CSV: csv.DictReader/DictWriter. TOML: tomllib. Always specify encoding='utf-8'.

#### J. Virtual Environments and Packages

Why isolation: dependency conflicts, reproducibility. venv, pip, uv, requirements.txt, pyproject.toml. Package manager comparison: pip, pipenv, poetry, uv.

#### K. Testing

pytest: discovery, assert, fixtures (@pytest.fixture, scope), parametrize, mocking (unittest.mock, patch, MagicMock), coverage (pytest-cov). Test organization: tests/, conftest.py.

#### L. Standard Library

os, sys, pathlib, subprocess (run, check=True, shell=True risks), argparse (subcommands), logging (levels, handlers), re, datetime (timezone-aware), collections, functools (lru_cache, partial, wraps), shutil.

#### M. Python Internals

Source → bytecode (.pyc) → CPython VM. GIL: what, why, threading implications. Memory: reference counting + generational gc. Threading vs. multiprocessing (concurrent.futures).

#### N. Project Structure

Modules, packages (__init__.py), entry points (if __name__ == '__main__', __main__.py). src/ vs. flat layout. .gitignore for Python.

### Exercises

- Build a CLI tool (argparse) that reads JSON, filters by criteria, outputs CSV — with full error handling and tests
- Implement a class hierarchy with dunder methods, a context manager, and a decorator — test with pytest
- Write a decorator that logs function calls with timestamps and arguments
- Create a virtual environment, install dependencies, freeze, recreate from scratch — demonstrate reproducibility
- Parse a log file with regex, aggregate with Counter, output a summary report
- Automate a system task with subprocess.run() — handle failures gracefully

### Proficiency Criteria

You write Pythonic code that passes ruff and mypy. You choose the right data structure and explain why (by complexity and access pattern). You test with pytest without thinking. You manage environments with venv as routine. You can read and understand any Python script you encounter.

---

## Domain 6. Data Structures and Algorithms

*Prerequisites: Domain 5*

### Learning Objectives

1. You can implement arrays, linked lists, stacks, queues, hash tables, trees, heaps, and graphs from scratch in Python.
2. You can analyze time and space complexity using Big-O, Big-Omega, and Big-Theta notation.
3. You can prove algorithm correctness using induction and analyze recursive algorithms with recurrence relations.
4. You can implement and analyze comparison sorts (merge, quick, heap) and non-comparison sorts (counting, radix).
5. You can implement BFS, DFS, Dijkstra's, and topological sort on graphs.
6. You can apply algorithm design paradigms: greedy, divide and conquer, dynamic programming, backtracking.
7. You can explain the practical implications of complexity for real systems.

### Topic Outline

*Mathematics is embedded throughout this domain — not as prerequisites, but as tools introduced at the point of need.*

#### A. Algorithm Analysis

**Math (embedded):** Asymptotic notation: Big-O, Big-Omega, Big-Theta — formal definitions and intuition. Best, worst, average case. Space complexity. Amortized analysis (aggregate, accounting, potential). Lower bounds and decision trees.

**Math (embedded):** Proof by induction — introduced here to prove algorithm correctness (e.g., that a loop invariant holds, that a recursive algorithm terminates).

**Math (embedded):** Recurrence relations — T(n) = 2T(n/2) + O(n) for merge sort, etc. Solving with substitution, recursion trees, Master Theorem.

**Practice:** Profile real code with `time`, `cProfile`, `timeit`. Compare O(n) vs. O(n log n) vs. O(n²) on actual data sets. Observe the difference between theoretical complexity and wall-clock performance (cache effects, constant factors).

**Connection:** Big-O is not an exam topic. It is the reason your API endpoint takes 30 seconds when the database table grows from 1,000 rows to 1,000,000. Understanding complexity tells you whether to optimize the code or add an index.

#### B. Fundamental Data Structures

Arrays: static vs. dynamic, how Python lists work (dynamic array with amortized O(1) append). Linked lists: singly, doubly, circular — implement insert, delete, search, reverse. Stacks: push, pop, peek — applications (expression evaluation, balanced parentheses, call stack). Queues: standard, circular, deque, priority queue. Hash tables: hash functions, collision resolution (chaining, open addressing), load factor, rehashing, amortized O(1).

**Math (embedded):** Probability — hash collision analysis. Given n keys and m buckets, the birthday problem tells you collision likelihood. Expected chain length under uniform hashing.

**Practice:** Implement each structure from scratch in Python. Benchmark against Python's built-in equivalents. Explain *why* dict lookup is O(1) — because it is a hash table with open addressing.

#### C. Trees

Binary trees: traversals (in-order, pre-order, post-order, level-order). BSTs: insert, delete, search, in-order successor. Balanced trees: AVL (rotations, balance factor), red-black (properties, recoloring). B-trees and B+ trees: disk-based storage, database indexes, branching factor. Heaps: min/max-heap, heapify, insert, extract, heapsort, priority queue implementation.

**Connection:** Red-black trees are not academic — Linux CFS uses one to track process virtual runtimes (you observed this in Domain 3). B+ trees are not academic — they are how PostgreSQL indexes work (you will use this in Domain 8). When you `EXPLAIN ANALYZE` a query and see "Index Scan using btree," this is what it means.

#### D. Graphs

**Math (embedded):** Graph theory formalized — vertices, edges, degree, paths, cycles, connectivity, directed vs. undirected, weighted vs. unweighted. Representations: adjacency matrix (O(V²) space, O(1) edge lookup), adjacency list (O(V+E) space), edge list.

BFS: queue-based, level-order, shortest path in unweighted graphs. DFS: stack/recursive, cycle detection, connected components. Topological sorting: Kahn's algorithm (BFS-based), DFS-based — requires DAG. Shortest path: Dijkstra's (non-negative, priority queue), Bellman-Ford (negative weights), Floyd-Warshall (all pairs). Minimum spanning tree: Kruskal's (union-find), Prim's (priority queue).

**Math (embedded):** Additional graph theory — coloring, planarity, Euler/Hamilton paths.

**Connection:** Terraform's dependency resolution is topological sort on a DAG. Kubernetes scheduler scoring is a graph problem. Network routing (OSPF) is Dijkstra's algorithm. When you understand graphs, you see them everywhere.

#### E. Sorting and Searching

Comparison sorts: bubble, selection, insertion (simple, O(n²)), merge (O(n log n), stable, extra space), quick (O(n log n) average, in-place, unstable), heap (O(n log n), in-place). Non-comparison sorts: counting, radix, bucket. Binary search and variants. Quickselect, median of medians. Stability, adaptivity, in-place properties.

**Math (embedded):** Prove the O(n log n) lower bound for comparison sorting using decision trees.

**Practice:** Implement merge sort and quicksort. Profile on different data distributions (sorted, reverse-sorted, random). Observe that insertion sort beats quicksort on small arrays (Python's Timsort exploits this).

#### F. Algorithm Design Paradigms

Brute force and exhaustive search. Greedy: activity selection, Huffman coding, fractional knapsack — proving greedy correctness (greedy choice property, optimal substructure). Divide and conquer: merge sort, closest pair — recurrence analysis. Dynamic programming: memoization vs. tabulation, optimal substructure, overlapping subproblems. Classic DP: 0/1 knapsack, LCS, edit distance, matrix chain multiplication, coin change. Backtracking: N-queens, Sudoku, constraint satisfaction.

**Math (embedded):** Combinatorics — permutations, combinations, pigeonhole principle, binomial theorem. Needed for counting states in backtracking and understanding combinatorial explosion.

**Connection:** Edit distance is not a textbook exercise — it is how `git diff` works (computing the minimum number of changes between two files). Dynamic programming is how network routing optimizes paths. Understanding algorithm design paradigms means you can solve problems you have never seen before.

### Exercises

- Implement a hash table from scratch with chaining, test with various load factors, measure collision rates
- Implement a BST with insert, delete, and in-order traversal — then implement AVL with rotations
- Implement Dijkstra's on a weighted graph, compare performance with Bellman-Ford
- Implement merge sort and quicksort, profile on datasets of size 100 to 1,000,000, graph the results
- Solve 5 DP problems: identify subproblems, write recurrence, implement both memoized and tabulated versions
- Given a real system problem (e.g., "find the shortest dependency chain in a package manager"), model it as a graph problem and solve it

### Proficiency Criteria

You can implement any fundamental data structure from scratch and analyze its complexity. You can explain *why* a hash table is O(1) and *when* it degrades. You can choose the right algorithm design paradigm for a novel problem. You can prove correctness with induction and analyze complexity with recurrence relations. You connect data structures to real systems: red-black trees in CFS, B+ trees in databases, DAG topological sort in Terraform.

---

## Domain 7. Networking

*Prerequisites: Domain 3*

### Learning Objectives

1. You can explain the OSI and TCP/IP models and troubleshoot by layer.
2. You can subnet IPv4 networks, calculate host ranges, and explain CIDR notation.
3. You can describe TCP (handshake, flow control, congestion control) and UDP, and capture both with tcpdump.
4. You can trace DNS resolution end-to-end and query every record type.
5. You can explain HTTP/1.1, HTTP/2, HTTP/3 including methods, status codes, and connection management.
6. You can describe TLS (handshake, certificates, cipher suites) and generate/inspect certificates with openssl.
7. You can configure SSH (key-based auth, config, tunneling) and harden it.
8. You can configure firewalls and explain routing, NAT, and overlays.
9. You can capture and analyze packets with tcpdump and Wireshark.
10. You can troubleshoot any connectivity issue systematically using layer-by-layer methodology.

### Topic Outline

#### A. Network Models and Fundamentals

**Theory:** Network types (LAN, WAN, MAN, PAN). Topologies (bus, star, ring, mesh). Circuit vs. packet switching. Bandwidth, latency, throughput, jitter. OSI (7 layers) and TCP/IP (4 layers). Encapsulation: data → segment → packet → frame. PDUs at each layer.

**Practice:** `ip link` (L2), `ip addr` (L3), `ss` (L4), `curl` (L7). Start with the model, then map each tool to its layer. This model is your troubleshooting framework.

#### B. Physical and Data Link Layer

**Theory:** Transmission media (copper, fiber, wireless). Signal encoding and modulation. Ethernet (802.3): frame format, CSMA/CD. MAC addressing (48-bit, OUI). Switches: MAC table, flooding, learning. VLANs (802.1Q): trunk/access ports. ARP: resolution, table, spoofing. Wi-Fi (802.11): standards, frequencies, WPA2/WPA3. Error detection: CRC, checksums. Flow control, sliding window. MTU, fragmentation, path MTU discovery.

**Practice:** `ip link show`, `ethtool`. Read ARP table: `ip neigh`. Create a VLAN: `ip link add link eth0 name eth0.100 type vlan id 100`. Check MTU: `ip link show | grep mtu`.

#### C. IP Addressing and Subnetting

**Math (embedded):** Binary arithmetic for subnetting. CIDR notation as a prefix length on a binary address.

**Theory:** IPv4 (32-bit, dotted-decimal). CIDR, variable-length subnet masks. Subnetting: network address, broadcast, host range. Private ranges (RFC 1918). Special addresses (127.0.0.0/8, 169.254.0.0/16). DHCP (discover, offer, request, acknowledge). IPv6 (128-bit, ::, link-local, SLAAC, DHCPv6). IP fragmentation, multicast, anycast.

**Practice:** Subnet a /24 into four /26 networks by hand: calculate network, broadcast, usable range. Verify with `ipcalc`. Configure a static IP with `ip addr add`. Read DHCP leases.

**Connection:** Subnetting is not a math exercise — it is how you know whether two machines can communicate directly or need a router. Every cloud VPC, every Kubernetes pod CIDR, every firewall rule depends on this.

#### D. Transport Layer

**Theory:** TCP: three-way handshake (SYN → SYN-ACK → ACK), sequence numbers, acknowledgments, flow control (sliding window, window scaling), congestion control (slow start, AIMD, fast retransmit, Cubic, BBR), teardown (FIN, TIME_WAIT), TCP options (MSS, SACK, timestamps). UDP: connectionless, unreliable, low overhead. Ports: well-known (0–1023), registered, ephemeral. Sockets. Multiplexing/demultiplexing.

**Practice:** Capture a TCP handshake with `tcpdump -i any -n port 80` — identify SYN, SYN-ACK, ACK in the output. Capture a DNS query (UDP) on port 53 and compare. Test connectivity with `nc` (netcat). Check listening ports with `ss -tlnp`.

**Connection:** When a connection hangs, understanding TCP states tells you whether the problem is the client (SYN sent, no SYN-ACK received → firewall or server down) or the server (TIME_WAIT accumulating → connection exhaustion).

#### E. DNS

**Theory:** Hierarchy: root → TLD → second-level → subdomains. Resolution: stub → recursive → root → TLD → authoritative. Record types: A, AAAA, CNAME, MX, NS, TXT, SRV, PTR, SOA, CAA. TTL and caching. Zones, zone files, zone transfers (AXFR/IXFR). DNSSEC, DoH, DoT. Split-horizon DNS.

**Practice:** `dig example.com A`, `dig +trace example.com` (full resolution path), `dig @8.8.8.8 example.com MX`, `dig -x 1.2.3.4` (reverse). Compare `nslookup` and `host`. Inspect `/etc/resolv.conf` and `/etc/nsswitch.conf`.

#### F. HTTP

**Theory:** HTTP/1.1 (text, persistent connections, pipelining). Methods: GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS — idempotency. Headers: Host, Content-Type, Authorization, Cache-Control, Cookie. Status codes: 2xx, 3xx, 4xx, 5xx (common codes). HTTP/2 (binary, multiplexing, HPACK, server push). HTTP/3 (QUIC/UDP, 0-RTT, no head-of-line blocking). Cookies, caching (ETag, Cache-Control). Email: SMTP, IMAP, POP3. FTP/SFTP.

**Practice:** `curl -v https://example.com` — read request and response headers, identify TLS negotiation, HTTP version, status code. `curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}'`.

#### G. TLS

**Theory:** Handshake: ClientHello → ServerHello → certificate → key exchange → encrypted. Certificate chain: root CA → intermediate → leaf. Validation: hostname, expiry, chain of trust, revocation (CRL, OCSP). Cipher suites: ECDHE (key exchange), RSA/ECDSA (auth), AES-GCM (encryption), SHA-256 (hash). TLS 1.2 vs. 1.3. mTLS: mutual authentication. Let's Encrypt, ACME.

**Practice:** `openssl s_client -connect example.com:443` — inspect the certificate chain. `openssl x509 -in cert.pem -text -noout` — read certificate details. Generate a self-signed cert: `openssl req -x509 -newkey rsa:4096`. Verify a chain: `openssl verify -CAfile ca.pem cert.pem`.

#### H. SSH

**Practice and theory combined:** Key exchange, authentication, encrypted channel. `ssh-keygen` (Ed25519 preferred). `~/.ssh/config` (aliases, ProxyJump, identity files). `ssh-agent`. Port forwarding: local (-L), remote (-R), dynamic/SOCKS (-D). scp, sftp, rsync over SSH. Hardening: disable password auth, disable root login, fail2ban. Bastion/jump hosts.

#### I. Routing

**Theory:** Routing table (destination, gateway, interface, metric). Default gateway. Static routes. Dynamic routing: distance vector (RIP), link state (OSPF — Dijkstra's algorithm, which you implemented in Domain 6), path vector (BGP — the protocol that runs the internet). NAT (SNAT, DNAT, PAT). Port forwarding. ICMP, traceroute (TTL expiry), mtr.

**Practice:** `ip route show`, `ip route add`. `traceroute`/`mtr` — interpret each hop.

**Connection:** OSPF uses Dijkstra's shortest-path algorithm. You implemented Dijkstra's in Domain 6. This is not a coincidence — the algorithm was designed for exactly this problem.

#### J. Network Overlays

**Theory:** Why overlays: VLAN limits (4096), multi-tenant isolation, container/VM mobility. VXLAN (UDP encapsulation, 24-bit VNI). GRE. IPsec (tunnel/transport mode, IKE). WireGuard. Software-defined networking (control/data plane separation).

#### K. Firewalls

**Theory:** Stateful vs. stateless. **Practice:** iptables (tables, chains, targets), nftables, ufw/firewalld. Cloud security groups (AWS SGs, Azure NSGs, GCP Firewall Rules — concept mapping).

#### L. Troubleshooting Methodology

Layer-by-layer: L1 (ip link, ethtool, cable) → L2 (ARP, VLAN) → L3 (ping, ip route, traceroute) → L4 (ss, nc) → L7 (curl, dig, app logs). tcpdump: -i, -n, -w (pcap), -r, BPF filters. Wireshark: capture vs. display filters, TCP stream following.

### Exercises

- Subnet a /24 into four /26 networks by hand, then verify with ipcalc
- Capture a TCP handshake and HTTP request with tcpdump, save to pcap, analyze in Wireshark, follow the stream
- Set up SSH key auth, configure ~/.ssh/config aliases, create a local port forward to access a remote service
- Trace DNS resolution with `dig +trace`, explain every step from root to authoritative
- Configure iptables: allow SSH + HTTP, default deny everything else, verify with nc
- Generate a self-signed CA, issue a leaf certificate, verify the chain with openssl
- Troubleshoot a deliberately broken network: identify the failing layer and fix it

### Proficiency Criteria

You troubleshoot by layer — you never start at L7 when the problem is at L3. You can read tcpdump output and identify what is happening at the packet level. You subnet in your head. You can explain TCP congestion control, DNS resolution, and the TLS handshake from memory. You understand that OSPF is Dijkstra's algorithm running on routers, and that VXLAN exists because VLANs ran out of address space.

---

## Domain 8. Data Management

*Prerequisites: Domain 7*

### Learning Objectives

1. You can explain the relational model and design schemas using entity-relationship modeling and normalization.
2. You can write SQL queries including joins, subqueries, aggregations, window functions, and CTEs.
3. You can explain normalization (1NF–BCNF) and make informed denormalization decisions.
4. You can explain ACID, transaction isolation levels, and concurrency control (2PL, MVCC).
5. You can explain B-tree indexes and use EXPLAIN to analyze query execution plans.
6. You can operate databases: backups, replication, migrations, connection pooling, monitoring.
7. You can explain CAP theorem and describe when to use relational vs. NoSQL databases.

### Topic Outline

#### A. The Relational Model

**Theory:** Tables, rows, columns, keys (primary, foreign, candidate, composite). Relational algebra: selection (σ), projection (π), join (⋈), union, difference. Relational calculus.

**Practice:** Create tables in PostgreSQL with primary and foreign keys. Insert data. Write SELECT queries that correspond to relational algebra operations.

**Connection:** Relational algebra is not abstract — it is what the query optimizer translates your SQL into. Understanding it helps you write better queries and read EXPLAIN output.

#### B. SQL

**Practice:** DDL (CREATE, ALTER, DROP). DML (SELECT, INSERT, UPDATE, DELETE). DCL (GRANT, REVOKE). TCL (BEGIN, COMMIT, ROLLBACK, SAVEPOINT). JOINs (inner, left/right/full outer, cross, self). Subqueries (scalar, table, correlated). Aggregation (GROUP BY, HAVING, COUNT, SUM, AVG). Window functions (ROW_NUMBER, RANK, LAG, LEAD, OVER/PARTITION BY). CTEs (WITH, recursive CTEs). Set operations (UNION, INTERSECT, EXCEPT).

#### C. Data Modeling and Normalization

**Theory:** Entity-relationship diagrams. Normalization: 1NF (atomic values), 2NF (no partial dependencies), 3NF (no transitive dependencies), BCNF (every determinant is a candidate key). Denormalization: when and why to break normal forms for read performance.

**Practice:** Design a schema for a real application. Normalize it step by step. Then identify a query that requires 5 joins and consider strategic denormalization.

#### D. Indexing

**Theory:** B-tree indexes (you implemented trees in Domain 6 — now you see them in production). Hash indexes. Composite indexes and the leftmost prefix rule. Covering indexes. Index selectivity.

**Practice:** Create indexes, run EXPLAIN ANALYZE, read execution plans. Identify full table scans. Add an index, observe the plan change, measure the speedup. Understand when indexes hurt (write-heavy workloads, low selectivity).

**Connection:** B+ trees from Domain 6 are not academic — they are literally what PostgreSQL uses for indexes. When EXPLAIN says "Index Scan using btree," you know the data structure underneath.

#### E. Transactions and Concurrency

**Theory:** ACID (atomicity, consistency, isolation, durability). Isolation levels: read uncommitted, read committed, repeatable read, serializable. Concurrency control: two-phase locking (2PL), timestamp ordering, MVCC. Deadlock detection in databases. Write-ahead logging (WAL).

**Practice:** Open two psql sessions. Demonstrate dirty reads by setting low isolation. Demonstrate a deadlock by locking rows in opposite order. Check pg_stat_activity for blocked queries.

**Connection:** Database deadlocks are the same four conditions from Domain 3 (OS deadlocks). The theory is identical — mutual exclusion, hold and wait, no preemption, circular wait. The tools differ, the diagnosis is the same.

#### F. Views, Stored Procedures, Triggers, Functions

Views (virtual and materialized). Stored procedures. Triggers. User-defined functions.

#### G. Practical Database Operations

PostgreSQL and MySQL/MariaDB as primary examples. SQLite for embedded. psql/mysql CLI. Backups: logical (pg_dump) vs. physical, point-in-time recovery. Replication: primary-replica, sync vs. async. Connection pooling (PgBouncer). Schema migrations: versioned, expand-contract, forward-only. Monitoring: slow query logs, pg_stat_statements.

#### H. NoSQL

Key-value (Redis, DynamoDB/Cosmos DB/Bigtable concepts). Document (MongoDB, Firestore). Column-family (Cassandra). Graph (Neo4j). CAP theorem: consistency, availability, partition tolerance — pick two. Eventual vs. strong consistency. When relational vs. NoSQL.

### Exercises

- Design a schema for an e-commerce system: normalize to 3NF, create in PostgreSQL, load sample data
- Write queries: multi-table joins, window functions, recursive CTE — explain each execution plan with EXPLAIN ANALYZE
- Create a B-tree index, measure before/after query performance, explain why it helped
- Demonstrate transaction isolation: create dirty read, phantom read, and deadlock scenarios across two sessions
- Set up primary-replica replication, write to primary, read from replica, verify lag
- Write a migration that adds a column with a default — explain why this can lock the table and how expand-contract avoids it

### Proficiency Criteria

You can design normalized schemas and make informed denormalization decisions. You read EXPLAIN output fluently and know when to add or remove indexes. You can demonstrate ACID properties and isolation levels with live transactions. You understand that B+ trees in databases, deadlocks in transactions, and CAP tradeoffs in distributed systems are not separate topics — they are the same computer science applied in different contexts.

---

## Domain 9. Security and Cryptography

*Prerequisites: Domains 7, 8*

### Learning Objectives

1. You can explain the CIA triad and map any security control to the principle it enforces.
2. You can explain symmetric encryption, asymmetric encryption, hashing, and digital signatures — including the math underneath.
3. You can describe PKI, generate and inspect certificates, and verify certificate chains.
4. You can explain authentication factors, MFA, OAuth 2.0, and OIDC.
5. You can explain authorization models (RBAC, ABAC, ACLs) and implement least privilege.
6. You can manage secrets through proper tooling and explain why hardcoded secrets are a critical vulnerability.
7. You can identify OWASP Top 10 vulnerabilities and explain their mitigations.
8. You can harden a Linux system following CIS Benchmark guidelines.
9. You can describe supply chain security, incident response, and compliance frameworks.

### Topic Outline

#### A. Security Principles

CIA triad. AAA. Least privilege. Defense in depth. Zero trust. Threat modeling (STRIDE). Risk assessment (likelihood × impact).

#### B. Cryptography

**Math (embedded):** Modular arithmetic — a mod n, modular exponentiation. Number theory — primes, GCD, LCM, Euclidean algorithm, Euler's totient function. These are taught here because they are the mathematical foundation of RSA and Diffie-Hellman.

**Theory:** Symmetric encryption: AES (128, 256), ChaCha20. Modes: ECB (insecure), CBC, GCM (authenticated). Asymmetric: RSA, ECDSA, Ed25519 — public/private keys. Hashing: SHA-256, SHA-3, BLAKE2 — properties (deterministic, collision-resistant, avalanche). Password hashing: bcrypt, scrypt, Argon2 (why general-purpose hashes are wrong for passwords). HMAC. Digital signatures. Key exchange: Diffie-Hellman, ECDHE. Randomness: /dev/urandom, CSPRNG.

**Practice:** Generate RSA and Ed25519 keys with `openssl`. Encrypt and decrypt a file with AES-256-GCM. Hash a file with `sha256sum`, modify one byte, observe the avalanche effect. **Then:** trace through RSA key generation using the number theory you just learned — p, q, n = pq, φ(n), e, d = e⁻¹ mod φ(n). The math is not abstract — it is what `openssl genrsa` just did.

**Math (embedded):** Bayes' theorem — taught here for reasoning about false positives in anomaly detection and intrusion detection systems. If your IDS has a 99% detection rate and a 1% false positive rate, but attacks are rare (1 in 10,000), most alerts are false positives. Understanding this prevents alert fatigue.

#### C. PKI and Certificates

Certificate authorities: root, intermediate, trust stores. X.509: subject, issuer, serial, validity, SAN. Certificate chain validation. Lifecycle: CSR → signing → distribution → renewal → revocation (CRL, OCSP). Let's Encrypt, ACME. Self-signed: when appropriate.

**Practice:** Generate a self-signed CA. Issue a leaf certificate. Verify the chain: `openssl verify -CAfile ca.pem cert.pem`. Inspect a live certificate: `openssl s_client -connect example.com:443`.

#### D. Authentication

Factors (knowledge, possession, inherence). MFA: TOTP, FIDO2/WebAuthn. Password security: length over complexity, managers, credential stuffing. SSO: SAML, OIDC. OAuth 2.0 vs. OIDC (authorization vs. authentication). Service authentication: API keys, mTLS, workload identity.

#### E. Authorization

RBAC, ABAC, ACLs. Policy engines: OPA, Cedar. Least privilege: scoped tokens, temporary credentials, JIT access. Cloud IAM concepts: AWS IAM, Azure RBAC, GCP IAM.

#### F. Secrets Management

What counts as a secret. Why hardcoded secrets are critical. Environment variables (better, still risky). Secret managers: Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager. Rotation. Secret scanning: git-secrets, truffleHog, GitHub scanning. Vault seal/unseal.

**Practice:** Set up Vault (dev mode). Store a secret. Retrieve it programmatically. Configure rotation.

#### G. OWASP Top 10

Injection (SQL, command — parameterized queries). Broken auth. Sensitive data exposure. XXE. Broken access control (IDOR). Misconfiguration. XSS (reflected, stored, DOM — output encoding, CSP). Insecure deserialization. Vulnerable dependencies. Insufficient logging. SSRF.

**Practice:** Exploit vulnerabilities in a deliberately vulnerable application (DVWA or Juice Shop). Then fix them.

#### H. System Hardening

Disable root SSH, key-based auth, firewall allow-list, remove unnecessary packages, automatic security updates, audit permissions, auditd, SELinux/AppArmor enforcing, sysctl hardening. CIS Benchmarks. Vulnerability scanners (Trivy, Lynis).

#### I. Supply Chain Security

Dependency scanning (npm audit, pip-audit, Dependabot, Trivy). SBOM (CycloneDX, SPDX). Artifact signing (Sigstore, cosign). Reproducible builds. SLSA framework.

#### J. Incident Response

Preparation → detection → containment → eradication → recovery → lessons learned. SIEM. Blameless post-mortems.

#### K. Compliance

SOC 2, ISO 27001, PCI DSS, HIPAA, GDPR — what each requires and why.

### Exercises

- Trace RSA key generation by hand: choose p and q, compute n and φ(n), choose e, compute d, encrypt and decrypt a number — then verify with openssl
- Generate a self-signed CA, issue certs, configure a service with TLS, inspect the chain
- Harden a fresh Linux install following CIS Benchmarks, document every change and why
- Exploit SQL injection and XSS in a vulnerable app, then implement the fixes
- Set up Vault, store and retrieve secrets programmatically, configure rotation
- Write an incident response runbook for a credential leak

### Proficiency Criteria

You can explain the math behind RSA and Diffie-Hellman, not just use them. You manage secrets through Vault, never hardcoded. You harden systems and explain every change. You can identify OWASP vulnerabilities in code and fix them. The CIA triad is not a definition to recite — it is the lens through which you evaluate every security decision.

---

## Domain 10. APIs and Integration

*Prerequisites: Domains 5, 7*

### Learning Objectives

1. You can explain REST constraints and evaluate whether an API is truly RESTful.
2. You can make API calls with curl and Python requests, handling authentication and errors.
3. You can parse and generate JSON/YAML with jq and Python.
4. You can authenticate with API keys, Basic auth, Bearer/JWT, and OAuth 2.0 flows.
5. You can implement retries with exponential backoff and explain idempotency.
6. You can describe GraphQL, gRPC, WebSockets, and webhooks — and when each is appropriate.
7. You can read OpenAPI specs and design a RESTful API.

### Topic Outline

#### A. What an API Is

Contract between components. Types: library, OS syscalls, web. Internal, partner, public.

#### B. REST

Constraints: client-server, stateless, cacheable, uniform interface, layered. Resources as nouns. HTTP methods → CRUD. Idempotency. HATEOAS. Richardson Maturity Model.

#### C. Data Formats

JSON syntax and pitfalls. YAML (Norway problem, implicit typing). XML (legacy, SOAP). Protocol Buffers. jq (.key, .[0], select, map, @csv, -r). yq.

#### D. Making API Calls

curl deep dive: -X, -H, -d, -v, -i, -L, -s, --data-binary. Python requests: get/post/put, params, json, headers, sessions, timeouts, raise_for_status(). httpie.

#### E. Authentication

API keys. Basic auth. Bearer/JWT (header.payload.signature, claims, expiration). OAuth 2.0 (authorization code, client credentials, device code, scopes, tokens). Service accounts.

#### F. API Design

Versioning (URL path, header, query). Pagination (offset, cursor, keyset). Rate limiting (429, backoff). Error responses. Idempotency keys. Bulk operations.

#### G. Client Resilience

Retries (5xx yes, 4xx no). Exponential backoff with jitter. Circuit breaker. Timeouts (connect vs. read).

#### H. Beyond REST

GraphQL (query language, single endpoint, N+1 problem). gRPC (protobuf, HTTP/2, streaming). WebSockets (full-duplex, real-time). Webhooks (callback, signature verification). SSE.

#### I. API Documentation

OpenAPI/Swagger. Reading specs. Swagger UI. Postman/Insomnia. API-first design.

### Exercises

- Interact with a public REST API via curl: CRUD operations with auth headers
- Write a Python client that paginates, handles 429 rate limiting with exponential backoff, and retries on 5xx
- Parse a complex nested JSON response with jq: extract, filter, transform to CSV
- Implement OAuth 2.0 client credentials flow in Python
- Design a RESTful API: resources, endpoints, methods, schemas, versioning, error responses

### Proficiency Criteria

You interact with any API given its documentation. Your clients have timeouts, retries, and error handling. You authenticate with any method. You parse JSON/YAML fluently. You know when REST, GraphQL, or gRPC is the right choice.

---

## Domain 11. Software Engineering and Collaboration

*Prerequisites: Domains 5, 9*

### Learning Objectives

1. You can explain Git's object model and use branching, merging, rebasing, and recovery fluently.
2. You can collaborate via pull requests, code review, and branching strategies.
3. You can explain design patterns and architectural patterns and apply them to real code.
4. You can design and implement CI/CD pipelines with build, test, scan, and deploy stages.
5. You can implement deployment strategies (rolling, blue-green, canary) and explain their tradeoffs.
6. You can describe GitOps principles and explain how they differ from push-based deployment.

### Topic Outline

#### A. Version Control (Git)

**Theory:** Git's object model: blobs, trees, commits, tags — the DAG. Snapshots, not diffs.

**Practice:** init, clone, add, commit, status, diff, log. Branching: switch -c, merge (fast-forward, three-way), conflict resolution, mergetool. Rewriting history: rebase, interactive rebase (pick, squash, fixup, reword), cherry-pick, revert, reset (--soft, --mixed, --hard). The golden rule: never rebase shared commits. Recovery: reflog, ORIG_HEAD, fsck. Remotes: fetch, pull (--rebase), push (--force-with-lease), tracking branches, forks. Collaboration: PRs, code review, branch protection, CODEOWNERS. Branching strategies: GitHub Flow, Git Flow, trunk-based. Advanced: .gitignore, stash, bisect, blame, hooks, pre-commit framework, worktree, submodule, signing, git-lfs.

#### B. Software Design

**Theory:** Modularity, coupling, cohesion. SOLID (revisited with experience — the learner has written thousands of lines now). Design patterns (GoF): Factory, Abstract Factory, Builder, Singleton, Prototype (creational). Adapter, Bridge, Composite, Decorator, Facade, Proxy (structural). Observer, Strategy, Command, Template Method, Iterator, State (behavioral).

**Connection:** Design patterns are solutions to problems you have already felt. Factory makes sense after you have written code that creates different objects based on input. Observer makes sense after you have wanted a component to react to changes in another. They are not abstract — they are names for things you have already done (or struggled to do).

#### C. Architecture

MVC, MVP, MVVM. Layered architecture. Clean architecture, hexagonal. Microservices vs. monolith (tradeoffs, boundaries, communication). Event-driven architecture. Build systems: Make, Gradle, Webpack, Vite (landscape awareness).

#### D. Code Quality

Code smells and refactoring. Static analysis and linters. Technical debt. Documentation: API docs, ADRs.

#### E. Testing Strategy

Testing pyramid: unit (many, fast) → integration (fewer) → E2E (fewest, slow). TDD: red, green, refactor. BDD: Given/When/Then. Test doubles: mocks, stubs, fakes, spies. Coverage: statement, branch, path. Property-based testing. Load testing. Chaos engineering concepts.

#### F. CI/CD

**Theory:** CI vs. CD vs. CD. Pipeline stages. The feedback loop.

**Practice (GitHub Actions as primary, others referenced):** Workflows (.github/workflows/*.yml). Triggers (push, pull_request, schedule, workflow_dispatch, workflow_call). Jobs, steps, runners. Matrix strategy. Caching (actions/cache). Artifacts. Secrets and environments (protection rules, reviewers). Reusable workflows, composite actions. Concurrency groups.

Pipeline design: build, test, lint, security scan (SAST, SCA, secret scanning), container build (tag with SHA + semver), artifact versioning. Quality gates: ruff, eslint, shellcheck, hadolint, trivy, semgrep, gitleaks, checkov, coverage thresholds. Required status checks.

**Other platforms:** GitLab CI, Jenkins, CircleCI — concept mapping. Cloud-native: AWS CodePipeline, Azure DevOps, GCP Cloud Build.

#### G. Deployment Strategies

Rolling (gradual), blue-green (atomic switch), canary (percentage routing), feature flags (deploy without activating). Database migrations in CI/CD (expand-contract). Rollback. Smoke tests.

#### H. GitOps

Declarative, Git as source of truth, automated reconciliation. Argo CD, Flux. Push vs. pull. Environment promotion. Drift detection.

#### I. Pipeline Security

Least-privilege runners. OIDC authentication (CI → cloud). Signed artifacts. Pin actions by SHA. PR security (fork restrictions, pull_request_target risks). Secret hygiene.

### Exercises

- Use interactive rebase to squash, reorder, and reword commits, then recover a "lost" commit via reflog
- Use git bisect with a test script to find the commit that introduced a bug
- Write a GitHub Actions workflow: build, test, lint, scan, matrix across Python versions, cache dependencies
- Implement environment-based deployments with manual approval for production
- Build and push a container image in CI, tagged with SHA and semver
- Compare the same pipeline in GitHub Actions and GitLab CI

### Proficiency Criteria

You use Git as a thinking tool. You rebase before merge, recover with reflog, and bisect to find bugs. You design CI/CD pipelines from scratch with proper testing, scanning, and deployment stages. You choose deployment strategies with informed reasoning. You can explain design patterns and apply them — not because you memorized them, but because you recognize the problems they solve.

---

## Domain 12. Infrastructure at Scale

*Prerequisites: Domain 11*

### Learning Objectives

1. You can explain how containers work at the kernel level and build optimized, secure images.
2. You can design multi-container applications with networking, storage, and health checks.
3. You can describe Kubernetes architecture and deploy, scale, update, and debug applications on it.
4. You can implement RBAC, network policies, and resource management in Kubernetes.
5. You can write Terraform configurations with modules, state management, and multi-environment patterns.
6. You can implement IaC in CI/CD with plan review, policy checks, and automated apply.
7. You can explain the distributed systems theory underneath: consensus, declarative reconciliation, state machines.

### Topic Outline

#### A. Container Fundamentals

**Theory:** Containers are namespaces + cgroups + overlay filesystem (you built this by hand in Domain 3). OCI spec: image, runtime, distribution. Container runtimes: containerd, CRI-O, runc, gVisor, Kata. Docker Engine vs. Podman (daemonless, rootless). Architecture: CLI → daemon → containerd → runc → container.

**Practice:** Build a Dockerfile: FROM, RUN, COPY, WORKDIR, ENV, USER, ENTRYPOINT, CMD. ENTRYPOINT vs. CMD (exec form vs. shell form). Layer model and cache optimization. Multi-stage builds. .dockerignore. Base images: full, slim, Alpine (musl), distroless, scratch. BuildKit: cache mounts, secret mounts, multi-platform builds. Image security: non-root USER, pin versions, scan (trivy, grype), sign (cosign).

Container lifecycle: run (-d, -it, --rm, --name), exec, logs, inspect, cp, stats, top. Resource limits: --memory, --cpus. Port mapping: -p host:container. Health checks.

Networking: bridge (default), host, none, custom bridge (DNS by name). Storage: volumes (managed, persistent), bind mounts (host-dependent), tmpfs.

Docker Compose: services, networks, volumes, depends_on (conditions), environment, profiles, watch.

Rootless containers: why they matter, Podman default, user namespace remapping.

Registries: Docker Hub, GHCR, Harbor, ECR/ACR/Artifact Registry. Tags vs. digests. Multi-arch manifests.

**Connection:** When you run `docker run`, Docker creates namespaces and cgroups — the same primitives you used with `unshare` in Domain 3. A container is not magic. It is the OS concepts you already understand, packaged.

#### B. Container Orchestration (Kubernetes)

**Theory:** Why orchestration: scheduling, scaling, networking, storage, self-healing. Declarative model: desired state → reconciliation → convergence.

**Theory (distributed systems):** etcd uses Raft consensus — a leader election protocol that ensures all nodes agree on state even when some fail. The API server is the single entry point. The reconciliation loop (observe → diff → act → repeat) is the core pattern. These are computer science concepts applied to infrastructure.

**Architecture:** Control plane: API server (REST, admission controllers), etcd (Raft consensus, source of truth), scheduler (filtering + scoring), controller manager (reconciliation loops), cloud controller manager. Worker nodes: kubelet, kube-proxy (iptables/IPVS), container runtime (CRI).

**Workloads:** Pod (smallest unit, shared network/storage). Multi-container patterns: sidecar, ambassador, adapter. Probes: liveness, readiness, startup (HTTP/TCP/exec). ReplicaSet. Deployment (rolling update, Recreate, rollout commands). StatefulSet (stable identity, stable storage, headless Service). DaemonSet. Job, CronJob.

**Networking:** ClusterIP, NodePort, LoadBalancer, ExternalName. Service mechanics (selectors, EndpointSlices). DNS (service.namespace.svc.cluster.local). Ingress (host/path routing, TLS termination). Ingress controllers (NGINX, Traefik). Gateway API. CNI (Calico, Cilium, Flannel). Pod-to-pod flat network. Network Policies (ingress/egress rules, default deny). Service mesh overview (Istio, Linkerd).

**Configuration:** ConfigMaps, Secrets (base64, encryption at rest, KMS), environment variables (env, envFrom, valueFrom). External Secrets Operator.

**Storage:** PV, PVC, StorageClass (dynamic provisioning), CSI drivers, volume snapshots.

**RBAC:** Role/ClusterRole (verbs + resources). RoleBinding/ClusterRoleBinding. ServiceAccounts. Pod Security Standards (Privileged, Baseline, Restricted). Pod Security Admission. Security contexts (runAsNonRoot, readOnlyRootFilesystem, capabilities). Admission controllers. OPA Gatekeeper, Kyverno.

**Resource management:** Requests (scheduling input), limits (max, OOMKill/throttle). LimitRange, ResourceQuota. QoS (Guaranteed, Burstable, BestEffort). HPA (scale on metrics). VPA. PDB.

**Scheduling:** Node selectors, affinity (node, pod), taints/tolerations, topology spread, priority/preemption.

**Operators and CRDs:** Custom Resource Definitions extend the API. Custom controllers reconcile. Operator pattern = CRD + controller. cert-manager, Prometheus Operator. Helm (charts, values, templates, releases).

**Troubleshooting:** kubectl (get, describe, logs, exec, port-forward, apply, delete, explain, diff, -o yaml/jsonpath). Debugging: events, ephemeral debug containers. Logging (Fluentd/Fluent Bit → Loki/Elasticsearch). Monitoring (Prometheus + Grafana, kube-state-metrics).

#### C. Infrastructure as Code (Terraform)

**Theory:** Declarative vs. imperative. Idempotency. Immutable infrastructure. The IaC landscape: Terraform/OpenTofu, Pulumi, CloudFormation, ARM/Bicep, CDK/CDKTF, Crossplane, Ansible.

**Theory (state machines and graph theory):** Terraform's dependency resolution is topological sort on a DAG (you implemented this in Domain 6). The plan/apply workflow is a state machine. State is a mapping from resource addresses to real-world IDs. These are CS concepts in production.

**Practice:** HCL syntax. Providers. Resources (CRUD lifecycle, implicit/explicit dependencies). Data sources. Variables (type, default, validation, sensitive). Outputs. Locals. Terraform blocks.

Plan/apply: init, validate, plan (+/-/~), apply, destroy. Refresh and drift detection.

State: JSON format, local vs. remote (S3+DynamoDB, Azure Storage, GCS), locking, state commands (list, show, mv, rm), security. Drift detection.

Expressions and functions: types, interpolation, conditionals, for expressions, splat, dynamic blocks. Functions: string, collection, filesystem, encoding, IP, type.

Meta-arguments: count, for_each, lifecycle (create_before_destroy, prevent_destroy, ignore_changes), moved block, import block.

Modules: structure, sources (local, registry, Git), inputs/outputs, versioning, composition.

Multi-environment: workspaces (limitations), directory-based (environments/dev, environments/prod), var-files, Terragrunt.

Testing and policy: validate, fmt, plan review, terraform test, Terratest, OPA/conftest, Sentinel, Checkov, tflint, Infracost.

CI/CD: lint → validate → plan → policy → approve → apply. Plan as PR comment. OIDC auth. Terraform Cloud, Atlantis. Drift detection.

Secrets: sensitive = true, state file protection, secret injection, data sources for secrets.

Advanced: provider development concepts, state internals, terraform graph, terraform console, backend migration, import workflow, refactoring with moved blocks.

### Exercises

- Build a multi-stage Dockerfile, optimize for cache hits, scan with trivy, reduce size by 50%+ with dive
- Write a compose.yaml for a three-service application with networking, volumes, health checks, and dependency ordering
- Deploy a multi-tier app on Kubernetes (kind): Deployments, Services, ConfigMaps, Ingress, network policies, HPA
- Configure RBAC: ServiceAccount with scoped Role, verify with `kubectl auth can-i`
- Debug a deliberately broken deployment: fix image pull, probes, resource limits, and service selectors
- Write Terraform for a complete environment: network, compute, storage — using modules, remote state, and for_each
- Set up CI/CD for Terraform: fmt check, validate, plan as PR comment, apply after approval
- Import an existing resource, write the config, verify plan shows no changes

### Proficiency Criteria

You can explain containers at the kernel level and build production-quality images. You describe every Kubernetes control plane component and explain the reconciliation loop. You implement RBAC and network policies from memory. You architect Terraform codebases for multi-environment teams. You understand the distributed systems theory underneath: Raft consensus in etcd, topological sort in Terraform, declarative reconciliation in controllers. You do not treat these tools as black boxes — you understand the computer science that makes them work.

---

## Domain 13. Theory of Computation

*Prerequisites: Domain 6*

### Learning Objectives

1. You can define DFAs and NFAs and prove their equivalence.
2. You can explain the correspondence between regular expressions and finite automata.
3. You can use the pumping lemma to prove a language is not regular.
4. You can describe pushdown automata and context-free grammars.
5. You can define Turing machines and explain the Church-Turing thesis.
6. You can prove the halting problem is undecidable.
7. You can explain P, NP, NP-completeness, and why it matters for real systems.

### Topic Outline

#### A. Automata Theory

**Theory:** DFA (definition, state diagrams, language recognition). NFA (nondeterminism, epsilon transitions). DFA-NFA equivalence (subset construction). Regular expressions ↔ finite automata equivalence. Pumping lemma for regular languages. Pushdown automata (PDA). Context-free grammars (CFG): production rules, derivations, parse trees. CFG-PDA equivalence. Pumping lemma for context-free languages. Chomsky and Greibach normal forms. Turing machines: definition, tape, head, transition function. Variants: multi-tape, nondeterministic, universal. Church-Turing thesis. Chomsky hierarchy: regular, context-free, context-sensitive, recursively enumerable.

**Connection to prior domains:** You have been using regular expressions since Domain 4. Now you learn that a regex defines a regular language, and a DFA is the machine that recognizes it. `grep` is, underneath, a finite automaton evaluator. Vim's modes are a finite state machine (you noted this in Domain 4). Context-free grammars describe programming language syntax — every time Python parses your code, it is using a CFG. This section formalizes things you have already been doing.

#### B. Computability

Decidable and undecidable languages. The halting problem: proof by diagonalization. Rice's theorem: all non-trivial semantic properties of programs are undecidable. Reducibility: mapping reductions, Turing reductions. Recursively enumerable and co-recursively enumerable.

**Connection:** The halting problem is not abstract philosophy. It is why no program can perfectly detect infinite loops, why perfect malware detection is impossible, and why static analysis tools produce false positives. Understanding undecidability tells you what is fundamentally beyond automation.

#### C. Complexity Theory

P, NP, co-NP. Polynomial-time reductions. NP-completeness. Cook-Levin theorem (SAT is NP-complete). Classic NP-complete problems: 3-SAT, Clique, Vertex Cover, Hamiltonian Cycle, TSP, Subset Sum. Approximation algorithms and heuristics.

**Connection:** Kubernetes scheduling is NP-hard (bin packing). The traveling salesman problem appears in network optimization. Constraint satisfaction appears in configuration management. When you recognize a problem as NP-hard, you know to reach for heuristics and approximations rather than wasting time searching for an exact polynomial-time solution. This is practical knowledge disguised as theory.

### Exercises

- Build a DFA (in Python or on paper) that recognizes a specific language, test it against strings
- Convert an NFA to a DFA using subset construction
- Prove that {aⁿbⁿ : n ≥ 0} is not regular using the pumping lemma
- Simulate a Turing machine that computes a simple function
- Walk through the diagonalization proof of the halting problem's undecidability
- Given a real system optimization problem, identify whether it is in P or is NP-hard, and explain the implications

### Proficiency Criteria

You can prove DFA-NFA equivalence. You can apply the pumping lemma. You can explain the halting problem proof and why it matters practically. You can identify NP-hard problems in real systems (scheduling, routing, configuration) and explain why heuristics are the right approach. You connect formal theory to the tools you have used throughout this path: regex to automata, grammar to parsers, complexity to system design decisions.

---

## Assessment Model

Every domain is assessed across three dimensions:

1. **Explain** — Write or present a technical explanation demonstrating theoretical understanding. You cannot fake knowing how page replacement works or why RSA depends on the difficulty of factoring large primes.

2. **Build** — Complete a project demonstrating practical skill. You cannot fake being able to deploy an application to Kubernetes or write a working CI/CD pipeline.

3. **Debug** — Troubleshoot a broken system where the fix requires first-principles reasoning, not just searching the error message. This is the bridge between theory and practice: given a misbehaving system, use your theoretical knowledge to reason about what is wrong and your practical knowledge to find and fix it.

The debug assessment is the most important. It is what separates understanding from memorization.

---

## Recommended Reading

### Foundational Texts

- Cormen, Thomas H., et al. *Introduction to Algorithms*. 4th ed., MIT Press, 2022.
- Sipser, Michael. *Introduction to the Theory of Computation*. 3rd ed., Cengage, 2013.
- Tanenbaum, Andrew S., and Herbert Bos. *Modern Operating Systems*. 4th ed., Pearson, 2015.
- Patterson, David A., and John L. Hennessy. *Computer Organization and Design*. 6th ed., Morgan Kaufmann, 2020.
- Kurose, James F., and Keith W. Ross. *Computer Networking: A Top-Down Approach*. 8th ed., Pearson, 2021.
- Silberschatz, Abraham, et al. *Database System Concepts*. 7th ed., McGraw-Hill, 2020.
- Gamma, Erich, et al. *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley, 1994.
- Rosen, Kenneth H. *Discrete Mathematics and Its Applications*. 8th ed., McGraw-Hill, 2019.

### Practical References

- Shotts, William. *The Linux Command Line*. 2nd ed., No Starch Press, 2019.
- Hightower, Kelsey, et al. *Kubernetes: Up and Running*. 3rd ed., O'Reilly, 2022.
- Brikman, Yevgeniy. *Terraform: Up and Running*. 3rd ed., O'Reilly, 2022.
- Kane, Sean P., and Karl Matthias. *Docker: Up and Running*. 3rd ed., O'Reilly, 2023.

### Online Resources

- MIT OpenCourseWare (ocw.mit.edu) — free CS course materials
- Khan Academy — mathematics foundations
- Kubernetes official documentation (kubernetes.io/docs)
- Terraform official documentation (developer.hashicorp.com/terraform)

---

*This integrated syllabus produces professionals who do not separate theory from practice,
because the real world never did. Every concept is grounded in something observable. Every
tool is explained by something fundamental. The result is deep expertise that compounds
over an entire career.*
