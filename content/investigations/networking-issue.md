---
title: "Intermittent VPN Drop: WireGuard MTU Fragmentation"
case_id: "CASE-115"
status: "RESOLVED"
category: "Networking & VPNs"
---

### Problem
Users connected to a remote internal environment via a WireGuard VPN tunnel experienced strange drops. SSH connections worked, but HTTP requests for large API payloads hung and timed out.

### Hypotheses
1. Packet loss in the routing firewall rules.
2. MTU size mismatch causing packet fragmentation across the network layer.
3. Cryptographic handshake timeout in the WireGuard interface key rotation.

### Evidence
Using `ping -M do -s 1400 [endpoint]` failed, while `ping -M do -s 1300 [endpoint]` succeeded, indicating packet drops for packets above a certain size threshold. Running `tcpdump -i wg0` confirmed that HTTP responses exceeding 1420 bytes were dropped at the gateway, with "Fragmentation Required" packets sent back but ignored by the underlying WAN router.

### Root Cause
The default WireGuard MTU was set to `1420` bytes. However, the external ISP route added extra encapsulation overhead (due to PPPoE and IPv6 tunnels), limiting the actual WAN path MTU to `1380` bytes. Packets exceeding `1380` bytes were dropped as fragmentation was disabled.

### Resolution
Set the interface MTU of `wg0` to `1360` bytes on both client and host sides in the WireGuard config file `/etc/wireguard/wg0.conf`. Configured TCP MSS clamping (`iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu`) to automatically optimize segment size transitions. Reinitialized the tunnel, resolving all HTTP timeouts.
