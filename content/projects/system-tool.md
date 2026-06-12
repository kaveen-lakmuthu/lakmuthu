---
title: "Custom Systems & Kernel Utilities"
description: "A collection of low-level systems utilities, custom operating system stubs, and kernel probe scripts."
category: "Personal Systems Project"
status: "ONGOING"
github: "https://github.com/kaveen-lakmuthu/system-tool"
last_updated: "2026-06-01"
date: "2026-06-01"
---

A collection of low-level systems utilities, custom operating system stubs, and kernel probe scripts:

### x86 OS Development
* **Objective**: Rebuilding an operating system kernel stub from the bootloader stage up.
* **Details**: Writing a custom 16-bit boot sector to bridge into 32-bit protected mode and 64-bit long mode. Implementing a basic monolithic kernel stub managing virtual memory paging, page directory tables, context scheduler loops, and keyboard driver interrupts.
* **Language**: x86 Assembly and C.

### C/C++ Network Sockets
* **Details**: Custom command-line tools utilizing raw TCP/IP sockets to audit network packets. Includes customized ICMP packet generators and local network traffic sniffing daemons.
* **Language**: C and POSIX shell APIs.

### Linux Kernel Probes (eBPF)
* **Details**: Writing kernel-level tracing scripts in eBPF to hook syscall boundaries. Monitors security state transitions, audits file I/O operations, and prints active scheduling latency distributions to sysfs files.
* **Language**: C and Python tracing scripts.
