---
title: "Debian Boot Failure: systemd Dependency Hang"
case_id: "CASE-109"
status: "RESOLVED"
category: "Linux Boot Sequence"
---

### Problem
Following a system upgrade on a Debian staging server, the system hung during boot phase, failing to reach the multi-user target. Access was limited to the emergency maintenance shell.

### Hypotheses
1. Corrupted file system block on the boot drive.
2. A misconfigured network mount block blocking systemd's local dependencies.
3. Broken device driver load loop during kernel initialization.

### Evidence
Running `journalctl -xb` revealed systemd was blocked waiting for `sys-subsystem-net-devices-eth0.device`. Checking network interfaces showed that after the kernel upgrade, the ethernet interface was renamed to `enp3s0` due to predictable interface naming conventions. However, the systemd network dependency configuration was still pointing to the legacy `eth0` target name.

### Root Cause
An auxiliary systemd unit (`network-online.target`) had a hard dependency on device unit `sys-subsystem-net-devices-eth0.device`. Because interface naming updated under the new kernel version, `eth0` never arrived, causing the boot process to wait indefinitely and timeout.

### Resolution
Edited the interface target configuration in `/etc/network/interfaces` to reflect `enp3s0`. Regenerated the default net-device targets, updated references in systemd overrides, and triggered a daemon reload. The reboot sequence succeeded cleanly in 12 seconds.
