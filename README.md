# NetworkWalks — Week 1 Project Module 1
## Cybersecurity Lab Setup: VirtualBox and Kali Linux

**Intern:** Huzaifa Waheed Khan  
**Internship ID:** NW-084-337  
**Program:** NetworkWalks Cybersecurity Internship  
**Week:** 1  
**Project Module:** PM1 — Lab Setup (VirtualBox and Kali Linux)

---

## 1. Project Overview

This project documents the setup of a cybersecurity practice lab using Oracle VirtualBox and Kali Linux.

The purpose of this lab is to prepare an isolated virtual environment for cybersecurity learning and practical exercises. Kali Linux is configured as the lab machine, connected through a VirtualBox NAT Network, with internet connectivity and a shared folder between the Windows host and the virtual machine.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Host Operating System | Windows |
| Virtualization Platform | Oracle VirtualBox 7.2.22 |
| Virtual Machine | Kali Linux 2026.3 |
| Guest RAM | 3 GB |
| Virtual CPUs | 2 |
| Network Adapter | NAT Network |
| NAT Network Name | Cybersecurity-Lab |
| Network CIDR | 10.0.0.0/24 |
| Kali Static IPv4 | 10.0.0.2/24 |
| Default Gateway | 10.0.0.1 |
| DNS Servers | 10.0.0.1, 1.1.1.1 |
| Shared Folder | Windows `C:\downloads` mounted at `/downloads` |

## 3. Network Configuration

A VirtualBox NAT Network named `Cybersecurity-Lab` was configured with the following settings:

- Network: `10.0.0.0/24`
- Gateway: `10.0.0.1`
- IPv4 address assigned to Kali: `10.0.0.2/24`
- Network interface: `eth0`

The Kali network connection was configured with a static IPv4 address using NetworkManager.

### Static IP Configuration

```bash
sudo nmcli connection modify "Wired connection 1" \
ipv4.method manual \
ipv4.addresses 10.0.0.2/24 \
ipv4.gateway 10.0.0.1 \
ipv4.dns "10.0.0.1 1.1.1.1"
```

A Duplicate Address Detection timeout compatibility adjustment was also applied:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The connection was restarted and successfully activated.

## 4. Connectivity Verification

The following tests were performed successfully:

| Test | Command | Result |
|---|---|---|
| Static IPv4 verification | `ip -4 addr show dev eth0` | Passed |
| Gateway connectivity | `ping -c 4 10.0.0.1` | 4/4 replies |
| External IP connectivity | `ping -c 4 1.1.1.1` | 4/4 replies |
| DNS resolution | `getent hosts kali.org` | Resolved successfully |
| Routing verification | `ip route` | Default route verified |
| HTTPS access | `curl -I https://www.kali.org` | Verified |
| Shared folder access | `ls -ld /downloads` | Verified |
| Shared folder write test | Create and remove test file | Verified |

## 5. Shared Folder Configuration

A Windows host folder was configured for sharing with Kali Linux.

- Host folder: `C:\downloads`
- Guest mount point: `/downloads`
- VirtualBox shared folder name: `downloads`
- Auto-mount: Enabled
- Permanent: Enabled

The Kali user belongs to the `vboxsf` group. Shared-folder access and file-write permissions were tested successfully.

## 6. Troubleshooting Experience

### Problem: Kali network connection failed to activate

During initial network configuration, the following error appeared:

```text
Error: Connection activation failed:
IP configuration could not be reserved
(no available address, timeout, etc.)
```

The Ethernet interface did not initially have an IPv4 address.

### Solution

The Duplicate Address Detection timeout was set to zero for the NetworkManager connection:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The connection was then restarted:

```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

After applying the adjustment, the connection activated successfully and Kali received the configured static address `10.0.0.2/24`.

### Verification

```bash
ip -4 addr show dev eth0
```

The output confirmed:

```text
inet 10.0.0.2/24
```

## 7. Virtual Machine Snapshot

A VirtualBox snapshot was created after completing the configuration and connectivity tests.

**Snapshot name:** `WK1-PM1 - Kali Linux Lab Setup Complete`

This snapshot preserves the configured lab state for future practical exercises.

## 8. Screenshots and Evidence

Screenshots documenting the completed setup are included in the repository's `screenshots` directory.

The evidence should cover:

1. VirtualBox Kali VM configuration.
2. VirtualBox NAT Network configuration.
3. Kali static IPv4 configuration.
4. Successful gateway ping.
5. Successful internet connectivity test.
6. DNS resolution.
7. Shared folder mounted at `/downloads`.
8. HTTPS connectivity test.
9. Successful VirtualBox snapshot.

## 9. Learning Outcomes

Through this project, I practised:

- Installing and configuring a Kali Linux virtual machine.
- Using VirtualBox NAT Network configuration.
- Configuring a static IPv4 address with NetworkManager.
- Verifying gateway and internet connectivity.
- Troubleshooting network activation issues.
- Configuring host-to-guest shared folders.
- Testing DNS and HTTPS connectivity.
- Creating a VirtualBox snapshot for recovery.

## 10. Conclusion

The Week 1 Project Module 1 lab setup was completed successfully. Kali Linux is running in VirtualBox with the required static IPv4 configuration, gateway access, internet connectivity, DNS resolution, shared-folder access, and a recovery snapshot.

This environment is prepared for subsequent cybersecurity internship practical exercises.

---

**Repository:** `NETWORKWALKS-Huzaifa-Waheed-Khan-NW-084-337-WK1-PM1-CYBERSECURITY-LAB-SETUP`  
**Internship ID:** NW-084-337  
**Program:** NetworkWalks Cybersecurity Internship
