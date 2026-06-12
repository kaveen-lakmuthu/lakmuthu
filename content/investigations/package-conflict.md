---
title: "Package Manager Lockup: Locked dpkg Process"
case_id: "CASE-112"
status: "RESOLVED"
category: "Package Management"
---

### Problem
An automated maintenance routine reported that the server package manager was completely locked. Any manual invocation of `apt install` or `apt-get` threw lock initialization errors and terminated immediately.

### Hypotheses
1. A parallel manual update process was left running in an unclosed screen session.
2. The automated `unattended-upgrades` system service hung during a database transaction.
3. Leftover stale lock files after an unexpected VM power cycle.

### Evidence
Running `lsof /var/lib/dpkg/lock-frontend` returned PID `29841`. Inspecting the process tree (`ps auxf -q 29841`) showed it belonged to an uncompleted `unattended-upgrades` script. Checking `/var/log/dpkg.log` showed that it was waiting on a user prompt block during a package upgrade transaction.

### Root Cause
The `unattended-upgrades` configuration did not have non-interactive variables defined properly for the target package, causing `dpkg` to prompt for config file differences. Because this was running in a headless background daemon, it stayed waiting for an input stream indefinitely.

### Resolution
Terminated process PID `29841` gracefully, removed stale locks, and reconfigured the unattended installer config file (`/etc/apt/apt.conf.d/50unattended-upgrades`) to force non-interactive config overrides (`Dpkg::Options::="--force-confold";`). Ran test dry-runs to verify it finished automatically.
