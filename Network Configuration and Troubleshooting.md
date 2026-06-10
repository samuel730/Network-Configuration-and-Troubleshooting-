# Critical Thinking Project: Network Configuration and Troubleshooting in Linux

## Project Overview

This project demonstrates real-world Linux network configuration and troubleshooting using a Vagrant Ubuntu 18.04 VM. It covers interface inspection, routing issues, DNS resolution, connectivity testing, and system repair.

---

# Objective

Learn how to configure and troubleshoot network settings in Linux.

By completing this lab, you will:

- Identify and analyze network interfaces
- Troubleshoot routing conflicts
- Restore internet connectivity
- Diagnose DNS resolution issues
- Apply Linux networking best practices

---

# Prerequisites

Before starting, ensure you have:

- Ubuntu 18.04 LTS (Vagrant VM)
- Sudo privileges
- Terminal access
- Internet connection (NAT adapter)

---

# Task 1: Network Interface Inspection

## Objective

Identify all active network interfaces.

---

## Command

```bash
ip a
```
### Observation

The system has two active interfaces:

- enp0s3 → NAT interface (10.0.2.15)
- enp0s8 → Host-only network (192.168.56.x)

Both interfaces were UP and operational.

##### Figure 1: Network Interface Configuration

<img width="1244" height="996" alt="image" src="https://github.com/user-attachments/assets/f5e5fd5f-dbe2-4f8e-9383-0b4e18556a16" />

## Task 2: Routing Table Analysis
### Objective

Check system routing configuration.

#### Command
```
ip route
```
##### Issue Identified

The system had two default routes:

- via 192.168.56.1 (incorrect)
- via 10.0.2.2 (correct NAT route)

This caused network failure and blocked internet access.

#### Figure 2: Routing Table Before Fix

<img width="1224" height="342" alt="image" src="https://github.com/user-attachments/assets/199f7ede-d9c3-4232-9f07-27932d1993ba" />

## Task 3: Network Connectivity Failure
### Objective

Test connectivity to the external network.

#### Commands
```
ping -c 4 8.8.8.8
ping -c 4 google.com
```
Problem Observed
- Ping to 8.8.8.8 failed (no connectivity)
- DNS resolution failed (google.com not resolved)
#### Figure 3: Network Failure Output

<img width="1266" height="606" alt="image" src="https://github.com/user-attachments/assets/080e46d9-3f6c-4d3e-b072-d928eb567e20" />

## Task 4: Routing Fix
### Objective

Fix the incorrect default gateway.

#### Fix Applied
```
sudo ip route del default via 192.168.56.1 dev enp0s8
```
#### Verification
```
ip route
```
Now only one correct route exists:
```
default via 10.0.2.2 dev enp0s3
```
#### Figure 4: Corrected Routing Table

<img width="1252" height="376" alt="image" src="https://github.com/user-attachments/assets/f4576cf4-51f5-4aca-8003-9d5a3d69bc8a" />

## Task 5: Network Recovery Test
### Objective

Verify restored connectivity.

#### Command
```
ping -c 4 8.8.8.8
```
#### Result
- Internet connectivity restored
- 0% packet loss
#### Command
```
ping -c 4 google.com
```
#### Result
- DNS resolution is working
- Domain successfully resolved
#### Figure 5: Successful Network Connectivity

<img width="1222" height="892" alt="image" src="https://github.com/user-attachments/assets/88562167-ead3-47d3-9b5b-c06a0ff1cb45" />

## Task 6: DNS Troubleshooting
### Objective

Verify DNS resolution using dig.

#### Command
```
dig google.com
```
#### Result

DNS resolved successfully using:
```
SERVER: 127.0.0.53
ANSWER: google.com → 172.217.22.78
```
#### Figure 6: DNS Resolution Output

<img width="1266" height="818" alt="image" src="https://github.com/user-attachments/assets/3d97046f-83fa-4666-888b-1e7466a95ae8" />

## Key Troubleshooting Actions Performed
#### 1. Network Diagnosis
- Checked interfaces using ip a
#### 2. Routing Analysis
- Identified multiple default gateways
#### 3. Problem Isolation
- Confirmed network failure via ping tests
#### 4. Root Cause Fix
- Removed incorrect route:
```
sudo ip route del default via 192.168.56.1 dev enp0s8
```
#### 5. Validation
- Restored internet connectivity
- Confirmed DNS resolution
### Final System Status
- Internet connectivity: Working
- DNS resolution: Working
- Routing configuration: Fixed
- Network interfaces: Active
## Conclusion

This lab demonstrates essential Linux networking troubleshooting skills used in DevOps environments. The key issue was a misconfigured routing table causing network failure. After correction, full connectivity and DNS resolution were restored successfully.
